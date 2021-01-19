---
title: '[Audio] 음성 데이터 자르기 (How to trim audio data with librosa)'
author: da-nyee
date: 2021-01-20 04:16:18 +0900
categories: [TIL, Audio]
tags: [audio, python, librosa]
---

## Generate Data

음성 데이터는 `Amazon Polly`, 즉 `TTS(Text-to-Speech)`를 이용하여 생성했다.<br/>

- Audio data: [South_Korean_national_anthem_lyrics.zip](https://github.com/da-nyee/da-nyee.github.io/files/5837488/South_Korean_national_anthem_lyrics.zip)
- File extension: mp3
- File length: 41secs
- Sampling rate: 22050Hz

## Convert Data

음성 데이터를 자를 때 `librosa` 패키지를 사용하는데, librosa로 `mp3`를 읽는 과정에서 이슈가 발생했다.<br/>

구글링을 해보니까 librosa로 mp3를 읽으려면 `ffmpeg` 패키지를 추가로 설치해야 됐다.<br/>

- Issue: [Not loading mp3 files.](https://github.com/librosa/librosa/issues/1075)
- Solution: [audioread and MP3 support](https://github.com/librosa/librosa#audioread-and-mp3-support)

나는 librosa로 wav, flac은 다뤄봤기 때문에 mp3를 `wav`로 바꾸는 게 빠르겠다 싶어 확장자를 변환시켰다.<br/>

- Audio data: [South_Korean_national_anthem_lyrics.zip](https://github.com/da-nyee/da-nyee.github.io/files/5837604/South_Korean_national_anthem_lyrics.zip)
- File extension: wav
- File length: 41secs
- Sampling rate: 96000Hz

## Write Code

음성 데이터는 `trim_audio_data` 함수를 이용하여 30초 길이로 잘랐다.<br/>

librosa로 음성 데이터를 로드할 때, `sampling rate`를 따로 설정하지 않으면 default로 sr=22500이 된다.<br/>
South_Korean_national_anthem_lyrics.wav 의 sampling rate는 96kHz이므로 `sr=96000`으로 지정했다.<br/>

추가적으로, 음성 데이터를 다른 길이로 자르고 싶다면 `sec` 변수에 원하는 값을 대입하면 된다.<br/>

- Audio data: [South_Korean_national_anthem_lyrics.zip](https://github.com/da-nyee/da-nyee.github.io/files/5838024/South_Korean_national_anthem_lyrics.zip)
- File extension: wav
- File length: 30secs
- Sampling rate: 96000Hz

```
import os
import librosa

import numpy as np

def trim_audio_data(audio_file, save_file):
    sr = 96000
    sec = 30

    y, sr = librosa.load(audio_file, sr=sr)

    ny = y[:sr*sec]

    librosa.output.write_wav(save_file + '.wav', ny, sr)

base_path = 'dataset/'

audio_path = base_path + '/audio'
save_path = base_path + '/save'

audio_list = os.listdir(audio_path)

for audio_name in audio_list:
    if audio_name.find('wav') is not -1:
        audio_file = audio_path + '/' + audio_name
        save_file = save_path + '/' + audio_name[:-4]

        trim_audio_data(audio_file, save_file)
```

## References

- [Online Audio Converter](https://online-audio-converter.com/)
- [음성 데이터 잘라보기](https://kaen2891.tistory.com/32)