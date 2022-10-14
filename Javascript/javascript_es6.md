# Javascript ES6

> 2015년에 Javascript 문법에 매우 큰 변화가 있었다. 바로 ES6(ES2015)의 등장이다.

## const, let

- ### var과 const, let의 차이

  기존에는 var로 변수를 선언했지만 var은 이제 const와 let으로 대체한다.

  ```javascript
  if (true) {
    var x = 3;
  }
  console.log(x); // 3

  if (true) {
    const y = 3;
  }
  console.log(y); // Uncaught ReferenceError: y is not defined
  ```

  var은 함수 스코프를 가지기 때문에 함수가 아닌 블록(이하`{}`) 안에서 정의를 해도 바깥에서 접근이 가능하지만 const, let은 블록 스코프를 가지기 때문에 if, while, for, function 등 `{}` 안에서 정의를 하면 바깥에서 접근을 할 수 없다. 덕분에 호이스팅 같은 문제도 해결되고 코드 관리도 수월해졌다.  
  **현 직장은 아직 IE 호환을 위해 var만을 사용한다.**

- ### const와 let의 차이

  const는 초기화시 값을 할당하지 않으면 에러가 발생하고 초기화 후에 다른 값을 할당하려고 할 때에도 에러가 발생한다. 따라서 const로 선언한 변수를 상수(변하지 않는 수)라고 부른다. let은 초기화시 값을 할당하지 않아도 에러가 발생하지 않고 초기화 후에 다른 값을 할당할 수도 있다.

  ```javascript
  const a = 0;
  a = 1; // Uncaught TypeError: Assignment to constant variable.

  let b = 0;
  b = 1;

  const c; // Uncaught SyntaxError: Missing initializer in const declaration
  ```

  실제로 Javascript를 사용하다보면 기존 변수에 값을 재할당하는 경우는 거의 없다. 따라서 기본적으로 const를 상용하되 값을 재할당할 필요가 있을 경우 let을 쓴다.

## 템플릿 문자열

```javascript
var name = "homin";
var weather = "맑음";
var string = name + "님 안녕하세요 오늘 날씨는" + weather + "입니다.";
```

기존에는 문자열과 변수를 합칠 때 위와같이 문자열과 변수 사이에 더하기 기호를 넣어서 해결했는데 이 방법은 더하기 기호 사용과 작은 따옴표와 큰 따옴표를 같이 쓸 경우 가독성이 매우 떨어진다. ES6부터는 백틱(`)을 사용하여 아래와 같이 사용할 수 있다.

```javascript
const name = "homin";
const weather = "맑음";
const string = `${name}님 안녕하세요 오늘 날씨는 ${weather}입니다.`;
```

변수는 ${변수}와 같은 형태로 사용하면 되고 위처럼 사용할 경우 더하기 기호를 사용할 필요도 없고 작은 따옴표, 큰 따옴표를 함께 사용할 수 있기 때문에 편리하다.

## 객체 리터럴

```javascript
var sayNode = function () {
  console.log("Node");
};
var es = "ES";
var oldObject = {
  sayJS: function () {
    console.log("JS");
  },
  sayNode: sayNode,
};
oldObject[es + 6] = "Fantastic";
```

```javascript
const sayNode = function () {
  console.log("Node");
};
const newObject = {
  sayJS() {
    console.log("JS");
  },
  sayNode,
  [es + 6]: "Fantastic",
};
```

위 두가지 코드를 비교하면 ES6에서 어떤 문법이 추가되었는지 알 수 있다. ES6에서는 위 객체의 `sayJS` 메서드에 함수를 연결할 때 콜론(:)과 함수 선언 예약어(function) 사용하지 않고 작성할 수 있다. 또한 `sayNode: sayNode`처럼 속성명과 변수 명이 동일한 경우 `sayNode`라고 한 번만 작성해도 된다. 마지막으로 객체의 속성명을 동적으로 생성해야할 경우 이전에는 객체 리터럴 바깥에서 `oldObject[es + 6] = "Fantastic"`를 해야 했으나 객체 리터럴 안에서 속성명을 동적생성 할 수 있게 되었다.

## 화살표 함수

**ES6 문법을 사용하지 못하는 현 직장에서 내가 가장 사용하고 싶어하는 문법**

```javascript
function add1(x, y) {
  return x + y;
}

const add2 = (x, y) => {
  return x + y;
};

const not = (x) => !x;
```

기존에 함수를 선언하던 예약어 대신 add2와 같이 화살표 기호(=>)를 사용하여 함수를 선언할 수 있다. 또한 함수 내부에 return문 밖에 없는 경우 not처럼 한 줄로 줄여 쓸 수도 있다. 익숙해지면 많은 코드를 줄여 쓸 수 있고 보기에도 깔끔하다. 단 주의해야 할 점이 한 가지 있다. 아래의 코드를 확인하자.

```javascript
var oldObject = {
  myName: "호민",
  friends: ["철수", "민수", "광수"],
  sayHello: function () {
    this.friends.forEach(function (friend) {
      console.log(this.myName + "의 친구 " + friend);
    });
  },
};
// undefined의 친구 철수
// undefined의 친구 민수
// undefined의 친구 광수

var newObject = {
  myName: "호민",
  friends: ["철수", "민수", "광수"],
  sayHello() {
    this.friends.forEach((friend) => {
      console.log(this.myName + "의 친구 " + friend);
    });
  },
};
// 호민의 친구 철수
// 호민의 친구 민수
// 호민의 친구 광수
```

화살표 함수는 함수 선언문(function)으로 선언한 함수와 this 스코프가 다르다. 기본 적으로 함수 내부에서의 this는 전역객체인 window를 가리키지만 화살표 함수 안에서의 this는 상위 스코프의 this를 상속받는다. 때문에 기본적으로 화살표 함수를 사용하되 this에 따라 함수 선언문과 화살표 함수를 선택해 사용하자.
