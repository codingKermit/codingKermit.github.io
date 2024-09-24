---
title: JQuery와 Easyui를 사용할 때 요소 사이즈가 0이 되는 문제
date: 2024-09-24
categories:
  - Error
tags:
  - javascript
  - jQuery
  - Easyui
---
안녕하세요 🐸  

저는 회사에서 EasyUI라는 라이브러리를 사용하고 있습니다.  

사용하는 곳이 많지는 않지만 혹시 몰라 남기기 위해 정리합니다.  

## 문제
- `hide()` 된 요소 내에서 EasyUI가 요소를 컨트롤 할 경우 요소의 size가 0px이 되어버림
## 버그 시점
- JQuery의 `hide()` 를 사용하여 `display:none` 처리 된 요소 내에서 EasyUI 라이브러리의 문법을 사용하여 요소를 조작할 때
## 원인
- EasyUI 라이브러리 자체에서 이러한 로직을 가지고 있었음.
  때문에 라이브러리 자체를 뜯어 고치지 않는 이상, 이 라이브러리를 쓰는 한 겪게 되는 문제
## 해결
- `hide()` 된 요소에서 EasyUI로 요소를 조작한 후, 해당 요소들의 사이즈를 원하는 사이즈로 조정하는 로직 추가

```javascript
	/* 조작하는 요소가 1개 이상이기 때문에 for 문을 사용 했습니다. */
	for(const id of ids){
		$(`#${id}`).combobox({
	        onLoadSuccess: function() { // 요소가 그려진 후 동작
	            $(`#${id}`).next().width('100%'); // 원하는 사이즈를 입력
	        }
		});
	}
```
