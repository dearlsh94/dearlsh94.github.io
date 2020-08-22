---
title: "javascript를 통한 DOM element 노드 제어하기"
date: 2019-09-03 00:00:00 -0400
categories: javascript
---



# 노드 추가

## appendChild()

- 새로운 노드를 해당 노드의 자식 노드 리스트(child node list)의 맨 마지막에 추가

```
function appendNode() {
    var parent = document.getElementById("list");  // 아이디가 "list"인 요소를 선택함.
    var newItem = document.getElementById("item"); // 아이디가 "item"인 요소를 선택함.

    parent.appendChild(newItem);                   // 해당 요소의 맨 마지막 자식 노드로 추가함.
}
```



## insertBefore()

- 새로운 노드를 특정 자식 노드 바로 앞에 추가
- 부모.insertBefore(신규, 자식);

```
function appendNode() {

    var parent = document.getElementById("list");           // 아이디가 "list"인 요소를 선택함.

    var criteriaItem = document.getElementById("criteria"); // 아이디가 "criteria"인 요소를 선택함.

    var newItem = document.getElementById("item");          // 아이디가 "item"인 요소를 선택함.

    parent.insertBefore(newItem, criteriaItem); // 해당 노드를 기준이 되는 자식 노드의 바로 앞에 추가함.

}
```



## insertData()

- 텍스트 노드의 텍스트 데이터에 새로운 텍스트를 추가
- 텍스트노드.insertData(오프셋, 새로운데이터);

```
var text = document.getElementById("text").firstChild; // 아이디가 "text"인 요소의 텍스트 노드를 선택함.

function appendText() {

    text.insertData(6, " 나른한 "); // 텍스트 노드의 6번째 문자부터 " 나른한 "이란 텍스트를 추가함.

}
```





# 노드 생성

## createElement()

```
function createNode() {

    var criteriaNode = document.getElementById("text"); // 기준이 되는 요소로 아이디가 "text"인 요소를 선택함.

    var newNode = document.createElement("p");          // 새로운 <p> 요소를 생성함.

    newNode.innerHTML = "새로운 단락입니다.";

    document.body.insertBefore(newNode, criteriaNode);  // 새로운 요소를 기준이 되는 요소 바로 앞에 추가함.

}
```



## createAttribute()

```
function createNode() {
	// 아이디가 "text"인 요소를 선택함.
    var text = document.getElementById("text");
    
	// 새로운 style 속성 노드를 생성함.
    var newAttribute = document.createAttribute("style");
    newAttribute.value = "color:red";
    
	// 해당 요소의 속성 노드로 추가함.
    text.setAttributeNode(newAttribute);
}
```



## createTextNode()

```
function createNode() {
	// 아이디가 "text"인 요소를 선택함.
    var elementNode = document.getElementById("text");
	// 새로운 텍스트 노드를 생성함.
    var newText = document.createTextNode("새로운 텍스트에요!");
	 // 해당 요소의 자식 노드로 추가함.
    elementNode.appendChild(newText);
}
```





# 노드 제거

## removeChild()

```
var parent = document.getElementById("list");      // 아이디가 "list"인 요소를 선택함.
var removedItem = document.getElementById("item"); // 아이디가 "item"인 요소를 선택함.

parent.removeChild(removedItem);                   // 지정된 요소를 삭제함.
```



## removeAttribute()

```
var text = document.getElementById("text"); // 아이디가 "text"인 요소를 선택함.

text.removeAttribute("style");              // 해당 요소의 "style" 속성을 제거함.
```





# 노드 복제

## cloneNode()

```
function cloneElement() {

    var parent = document.getElementById("list");     // 아이디가 "list"인 요소를 선택함.
    var originItem = document.getElementById("item"); // 아이디가 "item"인 요소를 선택함.
    
	// 해당 노드를 복제하여 리스트의 맨 마지막에 추가함.
    parent.appendChild(originItem.cloneNode(true));   
    // true -> 속성 노드 복제, 자식 노드 복제
    // false -> 속성 노드 복제

}		
```



# 노드 데이터 제어

	### setAttribute()

- element.setAttribute( 'attributeName', 'attributeValue' )

```
var para;

function changeAttribute() {
    // 모든 <p> 요소중에서 첫 번째 요소에 클래스 속성값으로 "para"를 설정함.
    document.getElementsByTagName("p")[0].setAttribute("class", "para");

    // 클래스가 설정되면 해당 클래스에 설정되어 있던 스타일이 자동으로 적용됨.
}
```



## nodeValue()

```
var para = document.getElementById("text"); // 아이디가 "text"인 요소를 선택함.

function changeText() {
    para.firstChild.nodeValue = "텍스트 변경 완료!";
}
```





# 노드 교체

## replaceChild()

- 기존의 요소 노드를 새로운 요소 노드로 교체
- 교체할노드 = 부모노드.replaceChild(새로운자식노드, 기존자식노드);

```
var parent = document.getElementById("parent"); // 부모 노드를 선택함.
var first = document.getElementById("first");
var third = document.getElementById("third");

function changeNode() {
	// first 요소를 삭제하고, 그 대신 third 요소를 삽입함.
    parent.replaceChild(third, first);
}
```



## replaceData()

- 텍스트 노드의 텍스트 데이터 교체	
- 텍스트노드.replaceData(오프셋, 교체할문자수, 새로운데이터);

```
var text = document.getElementById("text").firstChild; // 아이디가 "text"인 요소의 텍스트 노드를 선택함.

function changeText() {

    text.replaceData(7, 4, "저녁 7"); // 텍스트 노드의 7번째 문자부터 4개의 문자를 "저녁 7"로 교체함.

}
```





# 노드 관계 확인

```
var demodiv = document.getElementById("demodiv");				// 대상 노드 선택

var parent = demodiv.parentNode;								// 부모 노드 속성 확인
var children = demodiv.childNodes;								// 자식 노드 속성 확인
```

