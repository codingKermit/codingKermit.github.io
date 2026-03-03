---
title: 폰트를 못가져올 때 font-synthesis
date: 2026-03-03 13:21
categories:
  - 업무일기
tags:
  - CSS
_sort:
---
🐸분명 백엔드로 취업했지만 이것저것 다하는 개발구리입니다  
## 요약
- font-synthesis : 지정된 폰트에 bold, italic 등의 글꼴이 없을 때 브라우저가 해당 글꼴을 인위적으로 만들어 낼지 여부를 제어하는 속성
- 아래의 값을 가진다. (여러개를 가질 수 있다)
	1. `none`
	2. `weight`  
	3. `style` 
	4. `small-cap` 
	5. `position` 

## 배경 상황

오늘은 폰트를 변경해야 하는 작업이 발생했습니다  

🔈폰트를 Pretendard 로 바꿔주세요  

바꾸는 것 쯤이야 뭐 대수인가 그냥 바꾸는 작업은 후딱 끝났습니다만 아차! 실수를 해버렸네요  

cdn으로 폰트를 불러올 때에는 font-family와 font-weight 를 지정해서 가져오게 되는데 말이죠  

저는 700을 필요로 하는데 500을 가지고와 버렸네요!  

결국 알맞는 font-weight 를 찾지 못하고 제가 의도한 폰트를 가져오지 못했습니다.  

만! 어째서인지 bold체가 적용되어 보입니다.  

![뭘까](https://wsrv.nl/?url=https://media1.giphy.com/media/v1.Y2lkPTZjMDliOTUyZnEyYWNvYWt3dTgxZWdibXRhdjRyb2E2M2c0OXgwazlmcnl1a2JrYyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/l41JUepQ8rqpnI5ZS/giphy.gif&n=-1&w=200)  

## 원인
font-synthesis 속성이 존재하지 않는 weight 를 보여주기 위해 해당 폰트를 인위적으로 만들어내서 보여줬기 때문입니다.  

---
참고 : https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font-synthesis