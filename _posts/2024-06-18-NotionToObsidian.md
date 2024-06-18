---
title: 노션에서 옵시디언으로 옮기기
date: 2024-06-18
categories:
  - Obsidian
tags:
  - 노션
  - notion
  - 옵시디언
  - Obsidian
---
안녕하세요!

저는 노션에서 옵시디언으로 옮긴지 두 달이 되었네요

노션에서 메모해뒀던 자료들을 마이그레이션하기 귀찮아서 미루고 있었는데 최근 옵시디언에서 새  valuts를 열었다가 옵시디언에서 다른 플랫폼의 메모를 마이그레이션 하는 플러그인을 지원한다는 것을 알게 되었어요!

처음 valuts를 생성하면 아래와 같은 문구가 적힌 Welcome 파일이 생성되어있어요  

>This is your new *vault*.
>Make a note of something, [[create a link]], or try [the Importer](https://help.obsidian.md/Plugins/Importer)!
>When you're ready, delete this note and make the vault your own.

여기서 the importer 라는 문구가 눈에 들어와서 바로 확인을 해보니 옵시디언에서 제공하는 플러그인! 바로 사용해야죠

## 01. 노션 워크스페이스 내보내기

![](Pasted%20image%2020240618100034.png)  

설정과 멤버 -> 설정 -> 콘텐츠 내보내기 -> 내보내기 형식 : HTML

이때 ***주의할 점!!!*** 내보내기 형식을 마크다운으로 내보내지말고 HTML로 설정해서 내보내야합니다. 
저처럼 삽질하지 마세요 😭 문서에 있었는데 제가 안읽었었어요

## 02. importer 설치/활성화 하기
옵션 -> 커뮤니티 플러그인에서
`Importer` 플러그인을 설치하고 활성화 합니다.

![](/assets/img/screenshot/2024-06-17-NotionToObsidian/img1.png)  

(사실 옵시디언 사용자에게 이런 설명은 고등학생에게 숟가락 쥐는법 알려주는 수준이겠지만은...?)

## 03. Notion 파일 Import 하기

`Ctrl+P` 를 눌러 명령어 입력창을 열고 importer를 실행합니다.  

![](Pasted%20image%2020240617175034.png)
여기서 우리는 Notion을 선택합니다

![](Pasted%20image%2020240617175131.png)

Choose file -> 노션에서 내보내기한 zip 파일 선택
![](Pasted%20image%2020240617175831.png)  
알아서 파일 읽고 전환합니다 대박!
