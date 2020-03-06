---
title : "[CheatSheet] JavaScript"
excerpt : "자바스크립트"
category :
  - etc
tag :
  - etc
use_math : true
author_profile : true
header:
  teaser : /assets/images/main.jpg
  overlay_image : /assets/images/main.jpg
  overlay_filter: 0.1
---

## Output

```javascript
console.log(a); //콘솔창에 print
document.write(a); //html 문서
alert(a); //알림창
confirm("Really?"); //alert와 비슷한데 예,아니오
prompt("니 이름?","경수"); //alert와 비슷한데 주관식 답 입력
```

## 데이터타입

```javascript
typeof a // type of something
var age=18; //number
var name="kate"; //string
var dogs={first:"Mary",last:"Tom"}; //object
var tf=false; //boolean
var dogs=["Mary","Tom","James"]; //array
var empty; //undefined
var nothing=null; //value null
var f=function(){}; //function

const p=3.14; //constant
let z='string'; //local variable
```  
- false, true : boolean
- 1, 0.1, 0b10011, 0xF6, NaN : number
- "hallabong" : string
- undefined, null, Infinity : special
- var: 전역변수, 안과 밖에서 다 변함
- let: 블록내에서만 사용됨
- const: 새 설정 불가능, 변경은 가능

## 연산자  

```javascript
a=b //assignment
a!=b //not equal
a==b // equal
a===b // strict equal
a!==b // strict unequal
!(true) //logically not
a && b // logical and
a || b // logical or
```  
- Strict equality (===) is the counterpart to the equality operator (==). However, unlike the equality operator, which attempts to convert both values being compared to a common type, the strict equality operator does not perform a type conversion.

## function
```javascript
function myFunc(a,b){
  return a+b;
}
console.log(myFunc(1,2));//3

const myFunc2 = function(a,b){return a+b;}
console.log(myFunc2(2,2)); //4

const myFunc3 = (a,b) => {return a+b;}
console.log(myFunc3(3,2)); //5

var myFunc4 = (a,b) => a+b;
console.log(myFunc2(4,2)); //6
```

## Object
object
```javascript
var student = { // object name
    // list of properties and values
    firstName:"Jane", 
    lastName:"Doe",
    age:18,
    height:170,
    // object function
    fullName : function() {     
       return this.firstName + " " + this.lastName;
    }
}; 
student.age=19;
student[age]++;
name=student.fullName();
```
class
```javascript
class Book {
  constructor(author) {
    this._author = author;
  }
  get writer() {
    return this._author;
  }
  set writer(updatedAuthor) {
    this._author = updatedAuthor;
  }
  speak(){
    console.log(this._author+" speaks.");
  }
}
const lol = new Book('anonymous');
console.log(lol.writer);  // anonymous
lol.writer = 'wut';
console.log(lol.writer);  // wut
```
- getter: 이 함수를 호출하면 함수에서 지정한 변수 값을 리턴합니다.  
- writer: 이 함수를 호출하면 함수에서 지정한 변수 값을 수정합니다.

## 자료구조(Array)
```javascript
var dogs=["Poodle","Labrador","Shiba"];
var dogs=new Array("Poodle","Labrador","Shiba");
//modify
dogs[0]="Husky";
dogs.length; //3
dogs.toString() ; //"Husky, Labrador, Shiba"
dogs.join("*"); //Husky*Labrador*Shiba
dogs.pop(); //마지막 원소 제거
dogs.push("Pome"); //마지막 원소 추가
dogs.shift(); // 첫 원소 제거
dogs.unshift("Beagle"); //첫 원소 추가
delete dogs[1];
dogs.indexOf("Beagle");
dogs.splice(2,0,"Pug","Boxer"); //(시작점, 없앨원소개수, 추가하는 원소들)
dogs.slice(3) //(제거하는인덱스)
dogs.slice(2,4) //(시작점,끝점+1)
var animals=dogs.concat(cats,birds);
dogs.sort();
dogs.reverse();
```

## String
```javascript
var abc="catdogcatdog";
abc.length;
abc.indexOf("dog"); //3
abc.lastIndexOf("dog"); //9
abc.slice(1,4) //"atd"
abc.replace("dog","bird"); //catbirdcatdog
abc.toUpperCase();
abc.toLowerCase();
abc.concat(" ",str2);//abc+" "+str2
abc.charAt(2) //t
abc.split("");
```

## Loop
```javascript
for (var i=0; i<10; i++){...}

for (var i of cusrtOrder){...}

while(i<100){...}

do{...}while(i<100)

if(age<14)else{...}

switch(myValue){
  case 0:
    break;
  case 1:
    break;
  default:
    console.log('whatever');
}
```


## Reference
- <a href="https://htmlcheatsheet.com/js/">참고</a>