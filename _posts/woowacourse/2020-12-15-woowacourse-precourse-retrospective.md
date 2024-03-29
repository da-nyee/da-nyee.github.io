---
title: '[우아한테크코스] 우아한테크코스 프리코스 회고'
author: da-nyee
date: 2020-12-15 23:03:15 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, precourse, retrospective]
---

## <b>우아한테크코스란?</b>

[우아한테크코스](https://woowacourse.github.io/)는 [우아한형제들](https://www.woowahan.com/)에서 진행하는 프로그래밍 교육 과정입니다.<br/>

![techcourse_poster_3nd](https://user-images.githubusercontent.com/50176238/102137044-8fe6fe80-3e9d-11eb-9219-e55fbc9deb88.jpg)

10월 초에 3기 모집 소식을 [우아한형제들 기술 블로그](https://woowabros.github.io/)에서 접했고, 꼭 지원을 해야겠다고 생각했습니다.<br/>
11월 초에 서류를 접수하고 온라인 코딩 테스트를 응시했습니다.<br/>

![woowacourse](https://user-images.githubusercontent.com/50176238/102196694-960fc600-3f03-11eb-9f37-06ec97742650.PNG)

11월 중에 1차 심사 결과가 발표되었는데, 감사하게도 합격이었고 프리코스에 참여하게 되었습니다! 🙌<br/>

## <b>프리코스란?</b>

프리코스는 참가자에게 매주 미션이 주어지고 이를 요구사항에 따라 수행하는 과정입니다.<br/>

저는 11월 25일부터 3주 동안 <b>웹 백엔드</b> 분야로 참가하고 있고, 어느덧 마지막 미션을 완료했습니다.<br/>
총 3개의 미션을 수행하며 고민하고 생각했던 부분을 기록하기 위해 이번 포스팅을 작성합니다.<br/>

## <b>미션 1. 숫자 야구 게임</b>

첫번째 미션은 <b>숫자 야구 게임</b>이었습니다.<br/>

> 제가 작성한 1차 미션 코드는 [여기](https://github.com/da-nyee/java-baseball-precourse/tree/da-nyee)에서 확인하실 수 있습니다.<br/>

![baseballgame](https://user-images.githubusercontent.com/50176238/102199337-d9b7ff00-3f06-11eb-8864-4727bb2b909f.PNG)

처음 미션을 받았을 때, README를 읽으면서 익숙하지 않은 요구사항들에 약간 당황했습니다.<br/>
(전체 미션과 비교하면 높은 난이도의 미션은 아닙니다.)<br/>

특히, 프로그래밍 요구사항에 [자바 코드 컨벤션](https://google.github.io/styleguide/javaguide.html)을 지키라고 적혀 있었는데,<br/>
제가 평소에 고려하지 않았던 내용들이 있었고 제 지난 프로그래밍 습관을 반성하는 계기가 되었습니다.<br/>

또한, 진행 요구사항에 [깃 커밋 메시지 컨벤션](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)도 가이드 되어 있었습니다.<br/>
저는 주로 [Udacity 스타일](https://udacity.github.io/git-styleguide/)을 따라 커밋 메시지를 작성했는데,<br/>
이번 기회에 AngularJS 스타일을 알게 되고 적용하는 경험을 하게 되었습니다.<br/>

미션을 시작하기 앞서 <b>구현할 기능 목록</b>과 <b>확인할 프로그래밍 목록</b>을 작성했습니다.<br/>
구현할 기능 목록에는 프로그램 정상 흐름 외의 예외 처리 부분도 생각해서 정리했습니다.<br/>
확인할 프로그래밍 목록은 프로그래밍 요구사항과 자바 코드 컨벤션을 참고해서 정리했습니다.<br/>

1차 미션은 기능 구현이 크게 어렵지 않았습니다.<br/>
따라서, 얼른 기능을 완성하고 요구사항에 맞게 리팩토링하는 것에 집중했습니다.<br/>

1차 미션을 진행하며 가장 크게 신경썼던 부분은 <b>함수 분리</b>와 <b>상수 선언</b>입니다.<br/>

### <b>함수 분리</b>

먼저, 함수 분리는 프로그래밍 요구사항을 참고해서 진행했습니다.<br/>
요구사항에 <b>함수(또는) 메소드가 한 가지 일만 하도록 최대한 작게 만들어라.</b>라고 적혀 있어,<br/>
이 부분을 계속 생각하고 지키면서 코드를 작성했습니다.<br/>

![method_separation](https://user-images.githubusercontent.com/50176238/102203419-ea1ea880-3f0b-11eb-95b9-b2db0974b2d4.PNG)

함수 분리는 이런식으로 진행했습니다.<br/>
코드를 다시보니 <b>initflag()</b> 함수명을 축약해서 작성했는데.. 반성하게 됩니다.<br/>

### <b>상수 선언</b>

다음으로, 상수 선언은 Enum 클래스를 활용했습니다.<br/>
처음에는 상수를 클래스 상단에 선언하는 방식을 사용했습니다.<br/>
그런데, 기능을 구현하면서 코드 가독성이 떨어지는 것 같아 다른 방식을 고민하게 되었습니다.<br/>

괜찮은 방법이 없는지 구글링을 해보다가<br/>
우아한형제들 기술 블로그에서 [Java Enum 활용기](https://woowabros.github.io/tools/2017/07/10/java-enum-uses.html)를 읽게 되었습니다.<br/>
Enum에 대한 이해가 아직 부족해서 해당 포스팅의 전체 내용을 완벽히 숙지하지는 못했지만,<br/>
Enum을 활용해서 상수를 선언하면 되겠다! 라는 생각이 들게 해주었습니다.<br/>

![enum](https://user-images.githubusercontent.com/50176238/102206735-687d4980-3f10-11eb-96cf-8abc975626fc.PNG)

상수 선언은 이런식으로 Enum 클래스를 사용했습니다.<br/>
그동안 Java로 코딩하면서 Enum을 사용한 경험이 없어 네이밍을 어떻게 해야 하는지 고민하다가<br/>
<b>Type</b>이 가장 무난할 것 같아 <b>속해 있는 상수들을 일반화 할 수 있는 단어 + Type</b> 형태로 작성했습니다.<br/>

1차 미션의 기능 구현과 README 작성을 마치고,<br/>
프로그램을 전체적으로 테스트 한 후에 <b>Github PR</b>을 날려 미션을 완료했습니다! 😙

## <b>미션 2. 자동차 경주 게임</b>

두번째 미션은 <b>자동차 경주 게임</b>이었습니다.

> 제가 작성한 2차 미션 코드는 [여기](https://github.com/da-nyee/java-racingcar-precourse/tree/da-nyee)에서 확인하실 수 있습니다.<br/>

![racingcar](https://user-images.githubusercontent.com/50176238/102207920-13dace00-3f12-11eb-8d7a-e314e8607bd1.PNG)

2차 미션을 처음 받았을 때는 지난 1주일간의 경험 덕분인지 이전보다 익숙한 느낌이 들었습니다.<br/>

1차 미션보다는 난이도가 약간 올랐으나,<br/>
해당 미션도 시간 투자를 조금 더 한다면 금세 구현할 수 있는 수준이었습니다.<br/>

2차 미션 메일을 받았을 때, 2차 미션 안내와 함께 공통 피드백을 확인할 수 있었습니다.<br/>
2차 미션은 메일 내용을 바탕으로 <b>클래스 분리</b>, <b>네이밍</b>, <b>README 작성</b>에 집중했습니다.<br/>

### <b>클래스 분리</b>

먼저, 클래스 분리는 2차 미션 안내를 참고해서 진행했습니다.<br/>
미션 안내에 이번 미션의 목표가 적혀 있었는데, <b>함수 분리 + 클래스 분리</b>였습니다.<br/>
따라서, 이에 초점을 맞춰 한 클래스당 100라인이 넘지 않도록 코드를 작성하려고 노력했습니다.<br/>

![class_separation](https://user-images.githubusercontent.com/50176238/102211858-9914b180-3f17-11eb-9e58-6d55802c528c.PNG)

해당 프로젝트의 domain 패키지에 속한 클래스들입니다.<br/>
각 기능별로 클래스를 분리했고, 해당 클래스 내에서 함수를 작게 분리하며 코드 작성을 했습니다.<br/>

클래스 분리에 대해 아쉬운 점이 있는데,<br/>
오직 클래스 분리에만 초점을 맞추고 프로그램 구조는 신경쓰지 않은 점입니다.<br/>
코드를 다시 살펴보며 controller에 속해야 할 클래스가 domain에 속했다는 것을 알게 되었습니다. 😓<br/>

### <b>공통 피드백 - 네이밍</b>

다음으로, 공통 피드백에는 총 14개의 피드백이 있었습니다.<br/>
하나하나 완벽하게 숙지하기 위해 꼼꼼히 살펴봤습니다.<br/>

피드백 중 평소에 어렵다고 생각했던 네이밍과 README 작성에 대한 내용이 와닿았습니다.<br/>
따라서, 2차 미션에서는 의미를 더 직관적으로 전달하는 변수, 함수, 클래스명이 무엇일지 고민했고,<br/>
해당 프로젝트가 어떤 내용을 담고 있는지 전달하기 위해 README에 패키지와 파일 구조를 정리했습니다.<br/>

![naming](https://user-images.githubusercontent.com/50176238/102213067-869b7780-3f19-11eb-934f-4adc1a0000f8.PNG)

해당 프로젝트의 Winner 클래스에 속한 일부 함수들입니다.<br/>
우승자를 알아내기 위해 <b>checkWinner()</b> 함수를 작성했고,<br/>
우승자가 존재하는지 알아내기 위해 <b>isNoWinner()</b>와 <b>isWinner()</b> 함수들을 작성했습니다.<br/>

우승자를 알아내기 위한 부분을 3차 미션까지 끝낸 시점에 다시 확인하니 리팩토링할 부분이 보입니다.<br/>
지금은 <b>checkWinner()</b> 내에서 if 문으로 자동차 위치와 최대 위치를 비교하는 형태로 구현되어 있습니다.<br/>
2차 피드백을 참고하여, 객체에 메시지를 보내는 형태로 변경하면 더 좋은 코드가 될 것이라 생각합니다.<br/>

### <b>공통 피드백 - README 작성</b>

![directory](https://user-images.githubusercontent.com/50176238/102210430-81d4c480-3f15-11eb-9974-79e7c52bb11c.PNG)

해당 프로젝트의 README 일부 내용입니다.<br/>
이처럼 <b>완성된 디렉토리 구조</b>에 전체 패키지와 파일들을 트리 형태로 나타냈습니다.<br/>

README에 직접 디렉토리 구조를 그려본 건 처음이었는데,<br/>
한눈에 알아보기 좋은 형태인 것 같아 앞으로 종종 활용해 볼 생각입니다. 😄<br/>

2차 미션도 기능 구현과 README 작성을 마치고,<br/>
프로그램 전체 테스트를 한번 더 진행한 후에 <b>Github PR</b>을 날려 미션을 완료했습니다! 😚<br/>

## <b>미션 3. 지하철 노선도</b>

세번째 미션은 <b>지하철 노선도</b>였습니다.

> 제가 작성한 3차 미션 코드는 [여기](https://github.com/da-nyee/java-subway-map-precourse/tree/da-nyee)에서 확인하실 수 있습니다.<br/>

![subwaymap](https://user-images.githubusercontent.com/50176238/102218088-d5004480-3f20-11eb-95e8-ed139219964c.PNG)

3차 미션을 처음 받았을 때, 이제 프리코스가 1주 밖에 남지 않았다는 아쉬움이 들었습니다.<br/>
한편으로는 마지막 미션을 성공적으로 완성해서 유종의 미를 거두고 싶다는 생각도 했습니다.<br/>
(이번주에 있을 온라인 코딩 테스트가 진짜 마지막 미션이지만, 과제 형식으로는 마지막...!)<br/>

3차 미션은 지난 2주의 미션들보다 난이도가 많이 올라간 느낌이었습니다.<br/>
기능의 정상적인 흐름을 고려하는 것뿐만 아니라 예외 처리도 고려할 사항이 늘어났습니다.<br/>

1, 2차 미션은 하루 내로 구현할 기능 목록을 정리하고 다음날부터 바로 코드를 작성했는데,<br/>
3차 미션은 이틀 정도 고민을 하고, 초안을 작성하고, 또 고민을 했습니다.<br/>
중간에 설계도 자주 변경해서 깃 커밋 로그도 이전보다 2~3배 정도 늘어났습니다. 😂<br/>

3차 미션 메일을 받았을 때, 2차 미션과 마찬가지로 3차 미션 안내와 공통 피드백을 확인할 수 있었습니다.<br/>
3차 미션도 메일 내용을 바탕으로 목표를 세웠고,<br/>
이번에는 <b>클래스 관계 맺기</b>, <b>StringBuilder() 활용</b>, <b>상속 활용</b>에 집중했습니다.

### <b>클래스 관계 맺기</b>

먼저, 클래스 관계 맺기는 3차 미션 안내를 참고해서 진행했습니다.<br/>
미션 안내에 이번 미션의 목표는 <b>함수 분리 + 클래스 분리 + 클래스 관계 맺기</b>라고 적혀 있었습니다.<br/>
따라서, 이를 바탕으로 <b>MVC(Model-View-Controller) 패턴</b>을 활용해서 코드를 작성하려고 노력했습니다.<br/>

![mvc](https://user-images.githubusercontent.com/50176238/102218118-db8ebc00-3f20-11eb-9a60-a26804e1344b.PNG)

해당 프로젝트의 subway 패키지에 속한 패키지들과 클래스입니다.<br/>
MVC 패턴의 Model(domain), View, Controller를 기반으로 패키지, 클래스, 함수를 분리했습니다.<br/>

MVC 패턴을 몇 번 사용해봐서 나름 익숙하다고 생각했는데, 이번 미션을 진행하며 아님을 깨달았습니다.<br/>
개념적으로만 아는 것에 그치지 않고, 직접 더 사용해보면서 손으로 익혀야겠다고 다짐하게 되었습니다.<br/>

### <b>공통 피드백 - StringBuilder() 활용</b>

다음으로, 공통 피드백에는 1차 피드백과 동일하게 총 14개의 피드백이 있었습니다.<br/>
이번에도 모든 내용을 꼼꼼히 확인하고 모르는 내용은 검색하며 알아갔습니다.<br/>

전체 피드백 중에서<br/>
<b>객체의 상태를 보기 위한 로그 메시지 성격이 강하다면 toString()을 통해 구현한다.</b>는 내용이 있었는데,<br/>
이 부분이 잘 이해되지 않아 구글링을 반복했습니다.<br/>

피드백에 첨부된 코드와 검색 결과를 바탕으로,<br/>
조회 기능을 구현할 때 <b>StringBuilder()</b>를 사용해야겠다는 결론을 내리게 되었습니다.<br/>

![transitmap_show](https://user-images.githubusercontent.com/50176238/102219609-0aa62d00-3f23-11eb-90a9-154e34d66a96.PNG)

해당 프로젝트의 TransitMapShowService 클래스에 속한 일부 함수들입니다.<br/>
전체 지하철 노선의 상태를 확인하기 위해 <b>readTransitMap()</b> 함수를 선언했고,<br/>
객체를 출력 요구사항에 따라 출력하기 위해 StringBuilder()에 관련 값들을 append()해서 구현했습니다.<br/>

### <b>상속 활용</b>

마지막으로, 3차 미션에서 객체지향의 다형성을 살리고자 <b>상속</b>을 활용했습니다.<br/>
여러 클래스의 공통 함수는 <b>인터페이스</b>로 선언하고, 이를 구현 클래스에서 구현했습니다.<br/>
한 클래스에 중복되는 일부 함수는 <b>부모-자식</b> 관계로, 이를 자식 클래스에서 재정의했습니다.<br/>

![polymorphism](https://user-images.githubusercontent.com/50176238/102220956-dd5a7e80-3f24-11eb-82b5-45acddf3eec7.PNG)

해당 프로젝트의 StationService 클래스에 속한 일부 함수입니다.<br/>
<b>FeatureInterface()</b>에 추가, 삭제, 조회의 공통 로직이 추상 메소드로 선언되어 있습니다.<br/>
따라서, 이를 implements해서 현재 클래스에서 각 로직을 구현했습니다.<br/>
<b>SubwayService()</b>에는 기능 관리의 일부 공통 로직이 작성되어 있습니다.<br/>
따라서, 이를 extends로 상속받아 현재 클래스에서 해당 로직을 오버라이딩 했습니다.<br/>

3차 미션은 설계뿐만 아니라 구현도 생각보다 더 오랜 시간이 걸렸습니다.<br/>
사실 어제 새벽까지도 코드를 확인하고 리팩토링하는 과정을 거쳤습니다.<br/>
오늘 일어나자마자 코드와 구조를 마지막으로 정리하고, 문서를 작성했습니다.<br/>

이제 3차 미션도 기능 구현과 README 작성이 끝났으니,<br/>
프로그램 전체 테스트를 마지막으로 해 본 후에 <b>Github PR</b>을 날려 미션을 완료하겠습니다! 🤗<br/>

## <b>프리코스를 마무리하며</b>

프리코스가 끝나가는 시점에 포스팅을 하며 지난 3주를 되돌아볼 수 있었습니다.<br/>
또한, 이전 미션들을 복습하는 시간을 가졌습니다.<br/>

올해 Java를 자주 사용하고 Spring을 배우면서 Java는 어느정도 다룰 줄 안다고 생각했는데,<br/>
3주간 미션을 진행하고 다른 참가자분들의 코드를 보면서 아직 갈 길이 멀다고 느꼈습니다.<br/>

그러나, 여기서 포기하지 않고 앞으로 부족한 부분들을 채워가며 더 성장하는 개발자가 될 것입니다.<br/>
따라서, 최근에는 예전에 사용했던 Java 책을 다시 보며 공부하고 있습니다.<br/>

포스팅을 하며 이전 코드를 다시 살펴봤는데, 개선할 부분들이 꽤나 보입니다.<br/>
이번주의 두번째 온라인 코딩 테스트를 준비하며 재리팩토링하는 과정을 거쳐야겠습니다.<br/>

프리코스에서 좋은 코드란 무엇일까 고민하며 해답을 찾기 위해 노력한 과정이 재밌었고 보람찼습니다.<br/>
이번주에 있을 시험도 잘 응시해서 본 코스도 꼭 함께 하고 싶습니다! 🙌<br/>
본 코스에 참가해서 작성한 코드에 대한 개인 피드백도 받으며 더더욱 성장하고 싶습니다.<br/>

3주 동안 많이 배우고, 고민하고, 성장했습니다.<br/>
프리코스를 진행해주신 모든 분들께 감사드립니다! 😊<br/>