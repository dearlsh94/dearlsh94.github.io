---
title: "javascript를 통한 DOM 버튼 제어(추가,제거,이벤트)"
date: 2019-09-24 00:00:00 -0400
categories: javascript
---



# DOM 버튼(button) 제어

- 추가 / 제거 / 이벤트 할당 코드

```
var number = 0;
var body = document.getElementsByTagName("body");

var btnCreate = document.getElementById("btnCreate");
btnCreate.addEventListener("click", addButton);

function addButton() {
	var value = document.getElementById("buttonName").value;
  
	var newBtn = document.createElement("button");
  	newBtn.setAttribute("id", value);
  	newBtn.setAttribute("class", "button-01");
	newBtn.innerHTML = value;
  
  // alert 버튼 생성할 경우 이벤트 추가
  if (newBtn.id == "alert") {
  	newBtn.addEventListener("click", (e) => {
    	e.stopPropagation();
    	alertButton(value);
    });
  }
  
  body[0].appendChild(newBtn);
}

function alertButton(msg) {
	alert(msg);
}

var btnDelete = document.getElementById("btnDelete");
btnDelete.addEventListener("click", deleteButton);

function deleteButton() {
	var buttons = document.getElementsByTagName("button");
  
  for (i=0; i<buttons.length; i++ ){
  	if (buttons[i].id == "btnCreate" || buttons[i].id == "btnDelete")
    	continue;
    else {
    	console.log(buttons[i]);
    	buttons[i].parentNode.removeChild(buttons[i]);
      break;
    }
  }
}
/*
addButton(`btn${number}`, "Click", body);

function addButton(id, text, parentNode){
	var newBtn = document.createElement("button");
  newBtn.setAttribute("id", id);
	newBtn.innerHTML = text;
  
  parentNode[0].appendChild(newBtn);
}
*/

```

