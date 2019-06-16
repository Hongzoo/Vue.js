# 뷰 템플릿

## 뷰 템플릿이란?
HTML, CSS 등의 마크업 속성과 뷰 인스턴스에서 정의한 데이터 및 로직들을 연결하여 사용자가 브라우저에서 볼 수 있는 형태의 HTML로 변환해 주는 속성

템플릿 속성을 사용하는 방법 두 가지
1) ES5에서 뷰 인스턴스의 `template` 속성 활용
```html
<script>
  new Vue({
    template: '<p>Hello {{ message }}</p>'
  });
</script>
```
2) 싱글 파일 컴포넌트 체계의 `<template>` 코드 활용
```js
<template>
  <p>Hello {{ message }}</p>
</template>
```

## 템플릿에서 사용하는 뷰의 속성과 문법들
- 데이터 바인딩
- 자바스크립트 표현식
- 디렉티브
- 이벤트 처리
- 고급 템플릿 기법

### 데이터 바인딩
> 데이터 바인딩: HTML 화면 요소를 뷰 인스턴스의 데이터와 연결하는 것
- {{}} 문법
- v-bind 속성

#### {{}} - 콧수염 괄호
```html
<div id="app">
  {{ message }}
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      message: 'Hello Vue.js!'
    }
  });
</script>
```
> data 속성의 message 속성 값인 Hello Vue.js! 를 `<div>` 태그 안의 {{ message }}에 연결

위 코드에서 만약 data 속성의 message 값이 바뀌면 뷰 반응성에 의해 화면이 자동으로 갱신된다. 뷰 데이터가 변경되어도 값을 바꾸고 싶지 않다면 아래와 같이 `v-once` 속성을 사용.
```html
<div id="app" v-once>
  {{ message }}
</div>
```
> v-once 속성을 이용한 1회 바인딩

#### v-bind
아이디, 클래스, 스타일 등의 HTML 속성 값에 뷰 데이터 값을 연결할 때 사용.
```html
<div id="app">
  <!-- id, class, style 속성명 앞에 v-bind:를 붙인다 -->
  <p v-bind:id="idA">아이디 바인딩</p>
  <p v-bind:class="classA">클래스 바인딩</p>
  <p v-bind:style="styleA">스타일 바인딩</p>
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      idA: 10,
      classA: 'container',
      styleA: 'color: blue'
    }
  });
</script>
```

`v-bind:` 문법을 `:`로 간소화할 수 있다. ex) `v-bind:id` = `:id`  
하지만, 기본 문법과 약식 문법을 혼용해서 사용하지 않는 것이 좋으며 뷰 코드가 전반적으로 `v-`접두사를 붙이는 형태이기 때문에 가급적 `v-bind`속성을 이용하는 것이 기존 HTML 문법과 구분도 되고 다른 사람의 코드를 파악하기도 쉽다.

### 자바스크립트 표현식
데이터 바인딩 방법 중 하나인 {{}} 안에 자바스크립트 표현식을 넣으면 됨
```html
<div id="app">
  {{ message }} <!-- O, 그대로 출력 -->
  {{ message + "!!!" }} <!-- O, 문자열 조합 가능 -->
  
  {{ var a = 10; }} <!-- X, 선언문 사용 불가 -->
  {{ if (true) {return 100} }} <!-- X, 분기 구문 사용 불가 -->
  {{ true? 100 : 0 }} <!-- O, 삼항 연산자로는 표현 가능 -->
  
  {{ message.split('').reverse().join() }} <!-- △, 복잡한 연산은 인스턴스 안에서 수행 권장 -->
  {{ reversedMessage }} <!-- O, 스크립트에서 computed 속성으로 계산한 후 최종 값만 표현 -->
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      message: 'Hello Vue.js!'
    },
    computed: {
      reversedMessage: function() {
        return this.message.split('').reverse().join('');
      }
    }
  });
</script>
```
 message의 텍스트 값을 역순으로 변환하는 연산을 HTML 단에서 수행하지 않고, 자바스크립트 단에서 computed 속성을 이용하여 계산한 후 최종 결과 값만 표시를 권하는 이유?  
> 화면단 코드의 가독성을 높일 수 있으며, 반복적인 연산에 대해서는 캐싱(caching) 효과를 얻을 수 있다.

### 디렉티브 (Directive)
HTML 태그 안에 `v-`접두사를 가지는 모든 속성들을 의미.

**v-if**  
지정한 뷰 데이터의 true / false 에 따라 해당 HTML 태그를 화면에 표시하거나 표시하지 않음
```html
<div id="app">
  <a v-if="flag">두잇 Vue.js</a>
</div>
...
<script>
  new Vue({
    el: '#app',
    data: {
      flag: true
    }
  });
</script>
```
> 조건 값인 `flag` 값이 true 이므로 '두잇 Vue.js' 텍스트를 화면에 표시

**v-for**  
지정한 뷰 데이터 개수만큼 해당 HTML 태그를 반복 출력
```html
<div id="app">
  <ul>
    <li v-for="system in systems">{{ system }}</li>
  </ul>
</div>
...
<script>
  new Vue({
    el: '#app',
    data: {
      systems: ['android', 'ios', 'window']
    }
  });
</script>
```
> systems 배열의 요소 개수만큼 `<li>`태그가 반복되어 `{{ system }}`으로 각 요소의 값을 호면에 표시

**v-show**  
`v-if`와 유사하게 true / false 에 따라 화면에 표시여부를 결정. `v-if`는 해당 태그를 완전히 삭제하지만 `v-show`는 CSS 효과만 `display: none;`으로 주어 실제 태그는 남아있고 화면상으로만 보이지 않음.
```html
<div id="app">
  <a v-show="flag">두잇 Vue.js</a>
</div>
...
<script>
  new Vue({
    el: '#app',
    data: {
      flag: false
    }
  });
</script>
```
> `flag`값이 false 이므로 태그는 DOM요소로 살아있되, 화면상으로만 보이지 않는다.  
> `<a style="display: none;">두잇 Vue.js</a>` : 인라인 스타일 `display: none;`추가됨

**v-bind**  
HTML 태그의 기본 속성과 뷰 데이터 속성 연결
```html
<div id="app">
  <h5 v-bind:id="uid">뷰 입문서</h5>
</div>
...
<script>
  new Vue({
    el: '#app',
    data: {
      uid: 10
    }
  });
</script>
```
> uid 값인 `10`을 태그의 id 속성에 연결

**v-on**  
화면 요소의 이벤트를 감지하여 처리
```html
<div id="app">
  <button v-on:click="popupAlert">경고 창 버튼</button>
</div>
...
<script>
  new Vue({
    el: '#app',
    methods: {
      popupAlert: function() {
        return alert('경고 창 표시');
      }
    }
  });
</script>
```
> 버튼을 클릭했을 때 클릭 이벤트를 감지하여 methods 속성에 선언한 popupAlert() 메서드를 수행  
> (`v-on:click`은 해당 태그의 클릭 이벤트 감지를 의미)

**v-model**  
폼(form)에서 주로 사용되는 속성. 폼에 입력한 값을 뷰 인스턴스의 데이터와 즉시 동기화 함. `<input>`, `<select>`, `<textarea>` 태그에만 사용 가능.

### 이벤트 처리
`v-on` 디렉티브와 `methods` 속성을 활용하여 이벤트를 처리함
```html
<button v-on:click="clickBtn">클릭</button>
...
<script>
  methods: {
    clickBtn: function() {
      alert('clicked');
    }
  }
</script>
```
> `v-on` 디렉티브 이용해 이벤트 처리하기

```html
<button v-on:click="clickBtn(10)">클릭</button>
...
<script>
  methods: {
    clickBtn: function(num) {
      alert('clicked' + num + 'times');
    }
  }
</script>
```
> 인자 값 넘기기

```html
<button v-on:click="clickBtn">클릭</button>
...
<script>
  methods: {
    clickBtn: function(event) {
      console.log(event);
    }
  }
</script>
```
> `v-on:click`으로 호출하는 메서드에 인자를 전달하지 않아도 메서드 정의부에서 'event' 인자를 정의하면 해당 돔 요소의 이벤트 객체에 접근할 수 있음.  
> 실행 결과: 콘솔에 마우스 이벤트 객체가 출력된다.  

### 고급 템플릿 기법

**computed 속성**  
데이터를 가공하는 등의 복잡한 연산은 뷰 인스턴스 안에서 하고 최종적으로 HTML에는 데이터를 표현만 하기를 권하는데, computed 속성은 이러한 데이터 연산들을 정의하는 영역이다.

장점:  
> 1) computed 속성에서 사용하고 있는 data 속성 값이 변경도면 전체 값을 자동으로 다시 연산한다.
> 2) 캐싱  
  (methods속성과 다른 점: methods 속성은 호추할 때만 해당 로직이 수행되고 수행할 때마다 연산을 하기 때문에 별도로 캐싱을 하지 않는다.)

**watch 속성**  
데이터 변화를 감지하여 자동으로 특정 로직을 수행함. computed 속성과 유사하지만 computed 속성은 내장 API를 활용한 간단한 연산 정도로 적합한 반면에, watch 속성은 데이터 호출과 같이 시간이 상대적으로 더 많이 소모되는 비동기 처리에 적합.
```html
<div id="app">
  <input v-model="message">
</div>
...
<script>
  new Vue({
    el: '#app',
    data: {
      message: 'Hello Vue.js!'
    },
    watch: {
      message: function(data) {
        console.log("message의 값이 바뀝니다: ", data);
      }
    }
  });
</script>
```
> 실행결과: input박스의 입력 값을 v-model 디렉티브로 연결하여 입력 값에 변화가 있을 때마다 watch 속성에서는 변화된 값을 로그로 출력함  
> input박스에 초기값으로 `message: 'Hello Vue.js!'` 부분이 적용된다.
