# 컴포넌트 통신

## 컴포넌트 유효범위
컴포넌트마다 자체적으로 유효범위를 갖기 때문에 다른 컴포넌트의 값을 직접적으로 참조할 수 없다. 이렇게 다른 컴포넌트의 값을 참조하지 못하기 때문에 뷰에서 미리 정의해 놓은 데이터 전달 방식<sup>(1)</sup>에 따라 일관된 구조로 작성하게 된다. (뷰 프레임워크의 특징)  

* 장점
  - 개발자 개인적인 스타일대로 구성되지 않고, 동일한 흐름을 갖게되어 다른 사람의 코드를 빠르게 파악할 수 있다. (협업에 유익)

<sup>(1)</sup> :컴포넌트 간 데이터 전달 방식은 아래에서 설명.

## 컴포넌트 간 데이터 전달 방법
### 1. 상위(부모) 컴포넌트에서 하위(자식) 컴포넌트로 데이터 전달

```html
<div id="app">
  <!-- (2) child-component 태그에 v-bind 속성 추가 -->
  <child-component v-bind:propsdata="message"></child-component>
  <!-- v-bind:props속성이름="상위컴포넌트의 data속성"
      (해석) 상위컴포넌트에서 data속성값을 받아서 
      이 child-component의 props속성이름 값에 대입한다.
  -->
</div>
```
```js
Vue.component('child-component', {
  // (1) 하위컴포넌트의 속성에 props정의
  // props: 상위 컴포넌트에서 하위 컴포넌트로 데이터 전달할 때 사용하는 속성
  props: ['propsdata'],
  template: '<p>{{ propsdata }}</p>'
});

var app = new Vue({
  el: '#app',
  data: {
    message: '안녕 Vue! 상위 컴포넌트에서 만든 메시지야'
  }
});
```
See the Pen <a href="https://codepen.io/hongzoo/pen/eaKVXm/">
  props 속성을 사용한 데이터 전달 (상위 컴포넌트에서 하위 컴포넌트로)</a> by hong (<a href="https://codepen.io/hongzoo">@hongzoo</a>)
  on <a href="https://codepen.io">CodePen</a>.

### 2. 하위(자식) 컴포넌트에서 상위(부모) 컴포넌트로 이벤트 전달

```html
<div id="app">
  <child-component v-on:show-log="printAlert"></child-component>
  <!-- v-on:하위컴포넌트의 이벤트명="상위컴포넌트의 메서드명" -->
</div>
```
```js
// 전역 컴포넌트 생성
Vue.component('child-component', {
  template: '<button v-on:click="clickButton">show alert</button>', // v-on:이벤트명="메서드" -> 여기에서 메서드는 이 하위컴포넌트의 메서드
  methods: {
    clickButton: function() {
      // console.log(this); // 여기에서 this는 child-component 컴포넌트를 가리킨다.
      this.$emit('show-log'); // $emit()을 호출하면 괄호안에 정의된 이벤트 발생
    }
  }
});

var app = new Vue({
  el: "#app",
  methods: {
    printAlert: function() {
      alert("show-log 이벤트 발생시 나오는 얼럿");
    }
  }
});
```
See the Pen <a href="https://codepen.io/hongzoo/pen/mYKxbL/">
  이벤트를 발생시키고 수신하기 (하위 컴포넌트에서 발생 - 상위 컴포넌트에서 수신)</a> by hong (<a href="https://codepen.io/hongzoo">@hongzoo</a>)
  on <a href="https://codepen.io">CodePen</a>.
  
> ### 같은 레벨의 컴포넌트 간 통신하려면 
> 컴포넌트 고유의 유효 범위 때문에 다른 컴포넌트 값을 직접 참조하지 못하므로 기본적인 데이터 전달방식(상위에서 하위로만 데이터를 전달)을 활용해야 한다.  
> 하지만 이런 통신구조를 유지하다 보면 상위컴포넌트가 필요 없음에도 불구하고 같은 레벨간에 통신하기위해 강제로 상위 컴포넌트를 두어야 한다. 이를 해결할 수 있는 방법이 바로 `이벤트 버스`이다.

### 3. 관계 없는 컴포넌트간 이벤트 버스를 이용한 통신

```html
<div id="app">
  <child-component></child-component>
</div>
```
```js
// 이벤트버스로 활용할 새 인스턴스 생성.
var eventBus = new Vue(); // 이 eventBus의 .$emit과 .$on을 사용할 예정

Vue.component('child-component', {
  template: '<div>하위 컴포넌트 영역입니다.<button v-on:click="showLog">show</button></div>',
  methods: {
    showLog: function() {
      // 이벤트를 보내는 컴포넌트에서 .$emit 구현
      eventBus.$emit('triggerEventBus', 100); // 인자 값으로 100이라는 숫자 전달
    }
  }
});

var app = new Vue({
  el: '#app',
  created: function() {
    // 이벤트를 받는 컴포넌트에서 .$on 구현
    eventBus.$on('triggerEventBus', function(value){
      alert('이벤트를 전달받음. 전달받은 값:'+value);
    });
  }
});
```

See the Pen <a href="https://codepen.io/hongzoo/pen/BeXEra/">
  이벤트 버스 구현하기</a> by hong (<a href="https://codepen.io/hongzoo">@hongzoo</a>)
  on <a href="https://codepen.io">CodePen</a>.
  
> 단점: 컴포넌트가 많아지면 어디서 어디로 보냈는지 관리가 되지 않음.  
> 이 문제를 해결하려면 뷰엑스(Vuex)라는 상태관리 도구가 필요하지만 입문 레벨에서는 꼭 알아야 할 내용이 아니라고 한다.