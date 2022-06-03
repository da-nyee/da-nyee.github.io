---
title: '[DynamoDB] 코프링에서 DynamoDB 사용하기 (feat. spring-data-dynamodb)'
author: da-nyee
date: 2022-02-23 03:35:30 +0900
categories: [TIL, DynamoDB]
tags: [dynamodb, spring-data-dynamodb]
---

자프링에서 spring-data-jpa는 자주 써봤는데, spring-data-dynamodb는 아예 써본 적이 없다.<br/>

최근에 코프링에서 DynamoDB를 도입했다. 요런 조합이 처음이다보니 며칠 동안 삽질했다.<br/>
해결하고나서 뿌듯하면서도, 한편으로는 요런 경우가 또 있을 수도 있지 않을까 생각했다.<br/>
그래서 미래의 나를 위해 간단하게 정리한다.<br/>

<br/>

## 의존성 추가

- Spring에서 공식적으로 지원하는 spring-boot-dynamodb는 없다. 개인이 만든 라이브러리를 이용해야 한다.
- 아래의 `boostchicken` 프로젝트는 <b>AWS DynamoDB SDK</b>를 포함하고 있다.

```gradle
implementation "io.github.boostchicken:spring-data-dynamodb:5.2.5"
```

<br/>

## Datasource 설정

- 이때 다양한 지점에서 헤맸고 여러 오류를 마주했다.
- 그 중 하나는 DynamoDB 엔드포인트를 테이블 ARN으로 지정하면 될 줄 알았는데, 아니었던 것이다.<br/>
대신에 `https://dynamodb.ap-northeast-2.amazonaws.com`을 넣었더니 해결됐다.

```kotlin
// DynamoDbConfig.kt
@Configuration
@EnableDynamoDBRepositories(basePackages = ["레포지토리 패키지 경로"])
class DynamoDbConfig(
    @Value("\${dynamodb.accesskey}")
    private val accessKey: String,

    @Value("\${dynamodb.secretkey}")
    private val secretKey: String

    @Value("\${dynamodb.endpoint}")
    private val endPoint: String,

    @Value("\${dynamodb.region}")
    private val region: String
) {

    @Bean
    @Primary
    fun dynamoDbMapper(amazonDynamoDB: AmazonDynamoDB, dynamoDBMapperConfig: DynamoDBMapperConfig): DynamoDBMapper {
        return DynamoDBMapper(amazonDynamoDB, dynamoDBMapperConfig)
    }

    // EC2 인스턴스 프로파일에 DynamoDB 권한이 있는 경우
    @Bean
    @Primary
    fun amazonDynamoDB(): AmazonDynamoDB {
        return AmazonDynamoDBClientBuilder.standard()
            .withCredentials(InstanceProfileCredentialsProvider.getInstance())
            .withEndpointConfiguration(AwsClientBuilder.EndpointConfiguration(endPoint, region))
            .build()
    }

    // 설정 파일(e.g. application.yml)에 DynamoDB 권한이 있는 경우
    @Bean
    @Primary
    fun amazonDynamoDB(): AmazonDynamoDB {
        return AmazonDynamoDBClientBuilder.standard()
            .withCredentials(AWSStaticCredentialsProvider(BasicAWSCredentials(accessKey, secretKey)))
            .withEndpointConfiguration(AwsClientBuilder.EndpointConfiguration(endPoint, region))
            .build()
    }

    @Bean
    @Primary
    fun dynamoDBMapperConfig(): DynamoDBMapperConfig {
        return DynamoDBMapperConfig.DEFAULT
    }
}
```

<br/>

## 엔티티 또는 DTO 생성

### DynamoDB의 Primary Key

- 복합키가 아닌 경우
    - Partition Key 단독으로 쓴다.
- 복합키인 경우
    - Partition Key + Sorted Key 조합으로 쓴다.

### @DynamoDBHashKey와 @DynamoDBRangeKey

- `@DynamoDBHashKey`는 Partition Key에, `@DynamoDBRangeKey`는 Sorted Key에 붙인다.
    - 둘 다 무조건 <b>getter</b>에 붙여야 한다. 만약 필드에 붙인다면 에러가 발생한다.
- `attributeName = "필드명"`은 대소문자를 구분한다.
    - Partition Key와 Sorted Key가 대문자로 돼있다면 대문자를, 소문자로 돼있다면 소문자를 적어야 한다.

### @Id

- 복합키가 아닌 경우
    - `@DynamoDBHashKey` 바로 위에 붙인다.
- 복합키인 경우
    - 새로운 객체(e.g. UserId)를 만들고, 그 위에 붙인다.

```kotlin
// User.kt
@DynamoDBTable(tableName = "테이블명")
data class User(
    // `@get:`은 어노테이션을 getter에 붙이겠다는 의미이다.
    @get:DynamoDBHashKey(attributeName = "PK")
    var pk: String,

    @get:DynamoDBRangeKey(attributeName = "SK")
    var sk: String,

    @get:DynamoDBAttribute(attributeName = "age")
    var age: Int
) {

    @Id
    private var id: UserId? = null
        get() {
            return UserId(pk, sk)
        }
}

// UserId.kt
data class UserId(
    @DynamoDBHashKey
    var pk: String,

    @DynamoDBRangeKey
    var sk: String
)
```

<br/>

## Repository 작성

- JPA에서는 JpaRepository를 상속한다.
- 한편, DynamoDB에서는 <b>CrudRepository</b>를 상속하면 된다.

```kotlin
// UserRepository.kt
@EnableScan
interface UserRepository : CrudRepository<User, UserId>
```

<br/>

## 결론

이제 요구사항대로 DynamoDB Repository에 메시지를 보내 데이터를 CRUD하면 된다.<br/>

구글링을 하다보면 자프링 기반의 자료는 많았다.<br/>
근데 코프링 기반의 자료(특히 한글)은 적은 것 같아 하나 남겼다 !<br/>

<br/>

## References

- [AWS Docs - Java Code Examples](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/CodeSamples.Java.html)
- [AWS Docs - DynamoDB에 사용되는 Java 주석](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/DynamoDBMapper.Annotations.html#DynamoDBMapper.Annotations.DynamoDBAttribute)
- [[DynamoDB] Spring Data DynamoDB와 Embedded 개발 환경 구축하기](https://jojoldu.tistory.com/484)
- [Spring Boot에서 Repository로 DynamoDB 조작하기 (1) – 설정부터 실행까지](https://techblog.woowahan.com/2633/)
- [DynamoDB with Kotlin and Spring Boot (Part 1)](https://tuhrig.de/dynamodb-with-kotlin-and-spring-boot/)
- [(AWS DynamoDB) CRUDRepository 설정하기 - 복합키 Entity](https://overnodes.tistory.com/entry/AWS-DynamoDB-CRUDRepository)