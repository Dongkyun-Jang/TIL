# 👀Vue의 공식문서를 요약해보자

> 온전히 내 주관적인 요약이다.
>
> https://kr.vuejs.org/v2/guide/index.html

---

```html
<div id="app-2">
  <span v-bind:title="message">
    내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
  </span>
</div>
```

```javascript
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '이 페이지는 ' + new Date() + ' 에 로드 되었습니다'
  }
})
```

`v-bind` 속성은 디렉티브 라고 한다. 디렉티브는 Vue에서 제공하는 특수 속성임을 나타내는 `v- 접두어`가 붙어있으며, 렌더링 된 DOM에 특수한 반응형 동작을 한다. 

이 경우 `<span>` 태그의 `title` 속성은 `message`의 최신 상태로 유지된다. 즉, `data`의 `message`값을 수정하면 `span` 태그의 `title` 속성도 따라서 바뀌게 된다.

리액트의 `onChange` 함수와 비슷하다고 생각하면 좋을 것 같다. 

---

```html
<div id="app-3">
  <p v-if="seen">이제 나를 볼 수 있어요</p>
</div>
```

```javascript
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

seen이 true면 p 태그의 내용이 보이고, seen이 false이면 p 태그의 내용이 보이지 않는다.

이 예제는 텍스트의 속성뿐 아니라 DOM의 구조에도 데이터를 바인딩할 수 있음을 보여준다. 또한 Vue 엘리먼트가 Vue에 삽입/업데이트/제거될 때 자동으로 트랜지션 효과흫 적용할 수 있는 강력한 전환 효과 시스템을 제공한다.

---

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```javascript
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'JavaScript 배우기' },
      { text: 'Vue 배우기' },
      { text: '무언가 멋진 것을 만들기' }
    ]
  }
})
```

`v-for` 디렉티브를 활용하면 배열의 데이터를 바인딩하여 렌더링 시킬 수 있다.

---

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">메시지 뒤집기</button>
</div>
```

```javascript
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: '안녕하세요! Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

이거는 리액트의 `onClick()` 함수와 완전히 비슷한 형태이기 때문에 크게 헷갈리지 않을 것 같다.

---

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```javascript
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: '안녕하세요 Vue!'
  }
})
```

`v-model` 디렉티브를 사용하면 입력과 앱 상태를 양방향으로 바인딩할 수 있다. 이것도 결국에는 `onChange()` 함수와 비슷하다. `input` 태그로 사용자의 입력을 받아서 data 값을 수정해야할 때 자주 쓰이겠다.

---

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  }
})
```

Vue 인스턴스의 기본 모양. 

data 객체에 있는 모든 속성이 Vue의 반응형 시스템에 추가된다. 각 속성값이 변경될 때 뷰가 반응하여 리렌더링해준다.

결국 data 내부의 값들은 React Hook에서 useState로 관리하는 state들과 같은 기능을 가진다. 해당 값이 바뀌었을 때, 리렌더링되기를 기대한다면 해당 값은 data 객체 안에서 관리할 필요가 있겠다.

 하지만 유념해야할 점은, data에 있는 속성들은 인스턴스가 생성될 때 존재한 것들만 **반응형**이 된다는 것이다. 즉, 이미 app 인스턴스를 만든 이후에 `app.data['message2'] = 'bye'` 와 같이 데이터를 추가한다하더라도 이 속성은 반응형이 되지 못한다.

때문에 어떤 속성이 나중에 필요하다는 것을 알고 있다면, 빈 값으로 미리 data 객체 안에 넣어둘 필요가 있다.

```javascript
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

---

![Vue_LifeCycle](../assets/img/Vue_LifeCycle.png)

각 Vue 인스턴스는 생성될 때 일련의 초기화 단계를 거친다. 예를 들어, 데이터 관찰 설정이 필요한 경우, 템플릿을 컴파일하는 경우, 인스턴스를 DOM에 마운트하는 경우, 그리고 데이터가 변경되어 DOM를 업데이트하는 경우가 있다. 그 과정에서 사용자 정의 로직을 실행할 수있는 **라이프사이클 훅** 도 호출됩니다. 예를 들어, [`created`](https://kr.vuejs.org/v2/api/#created) 훅은 인스턴스가 생성된 후에 호출됩니다. 예:

```javascript
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 는 vm 인스턴스를 가리킵니다.
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

---

```html
<span>메시지: {{ msg }}</span>
```

데이터 바인딩의 가장 기본 형태는 "Mustache" 구문(이중 중괄호)을 사용한 텍스트 보간이다.

지금까지는 간단한 데이터만 바인딩 했지만, 실제로는 다음과 같은 JavaScript 표현식도 가능하다.

```javascript
// 가능한 구문
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>

// 단일 표현식이 아니기 때문에 불가능한 경우
{{ var a = 1 }}
{{ if (ok) { return message } }}
```

---

가장 자주 사용되는 두 개의 디렉티브인 `v-bind`와 `v-on`에 대해서는 특별한 약어를 제공한다.

```html
<!-- 전체 문법 -->
<a v-bind:href="url"> ... </a>

<!-- 약어 -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
```

```html
<!-- 전체 문법 -->
<a v-on:click="doSomething"> ... </a>

<!-- 약어 -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

(지금 상황에서 dynamic argument는 잘 모르겠다. 나중에 더 공부해서 이 부분을 다시 확인할 필요가 있겠다.)

---

```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

템플릿 내에 표현식을 넣으면 편리하지만, 너무 많은 연산을 템플릿 내에서 하면 코드가 비대해지고 유지보수가 어려워진다. 때문에, 우리는 **computed 속성**을 사용해야 한다.

```html
<div id="example">
  <p>원본 메시지: "{{ message }}"</p>
  <p>역순으로 표시한 메시지: "{{ reversedMessage }}"</p>
</div>
```

```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: '안녕하세요'
  },
  computed: {
    // 계산된 getter
    reversedMessage: function () {
      // `this` 는 vm 인스턴스를 가리킵니다.
      return this.message.split('').reverse().join('')
    }
  }
})
```

연산은 이렇게 computed 속성 내의 함수로 구분하는 것이 좋다.

`reversedMessage()` 함수는 내부에서 `message` 값을 활용하고 있다. 때문에 `message`가 바뀔 때 `reversedMessage()`에 의존하는 바인딩을 모두 업데이트 하게 된다. 

---

표현식에서 메서드를 호출하여 같은 결과를 얻을 수도 있다.

```html
<p>뒤집힌 메시지: "{{ reversedMessage() }}"</p>
```

```javascript
// 컴포넌트 내부
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

위의 경우와 다른 점은 크게 2가지이다. `reversedMessage`가 아닌 `reversedMessage()`라는 점, `computed`가 아닌 `methods`라는 점.

일단 두 가지 접근 방식은 같은 결과를 도출한다. 차이점은 `computed` 속성은 종속 대상을 따라 저장(캐싱)된다는 것이다. `computed` 속성은 해당 속성(`reversedMessage 함수`)이 종속된 대상(`message`)이 변경될 때만 함수를 실행한다. 즉, `message`가 변경되지 않는 한, `computed` 속성인 `reversedMessage`를 여러 번 호출해도 계산을 하지 않고 계산되어 있던 결과를 반환한다.

`methods` 속성인 `reversedMessage`는 호출될 때 값이 바뀌고 그 값을 반환한다.

---

