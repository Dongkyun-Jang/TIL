# 💞정적 스코프 vs 동적 스코프

> 모던 자바스크립트 딥 다이브(위키북스)의 내용입니다.

---

### 1. 동적 스코프

> 함수를 어디서 호출했는지에 따라 함수의 상위 스코프를 결정한다.

### 2. 렉시컬 스코프(정적 스코프)

> 함수를 어디서 정의했는지에 따라 함수의 상위 스코프를 결정한다.

기본적으로 자바스크립트는 렉시컬 스코프를 따른다.

---

```javascript
var x = 1

function foo() {
	var x = 10
	bar() 
}

function bar() {
	console.log(x)
}

foo() //?
bar() //?
```

이 자바스크립트 코드의 결과는 어떻게 될까?

바로 전역 변수 x의 값 1을 두 번 출력하게 된다.

자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다. 함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향도 주지 않는다. 즉, 함수의 상위 스코프는 언제나 자신이 정의된 스코프이다.

이처럼 함수의 상위 스코프는 함수 정의가 실행될 때 정적으로 결정된다. 함수 정의(함수 선언문 또는 함수 표현식)가 실행되어 생성된 함수 객체는 이렇게 결정된 상위 스코프를 기억한다. 함수가 호출될 때마다 함수의 상위 스코프를 참조할 필요가 있기 때문이다.