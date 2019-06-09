# 컴포넌트 통신

## 컴포넌트 유효범위
컴포넌트마다 자체적으로 유효범위를 갖기 때문에 다른 컴포넌트의 값을 직접적으로 참조할 수 없다. 이렇게 다른 컴포넌트의 값을 참조하지 못하기 때문에 뷰에서 미리 정의해 놓은 데이터 전달 방식<sup>(1)</sup>에 따라 일관된 구조로 작성하게 된다. (뷰 프레임워크의 특징)  

* 장점
  - 개발자 개인적인 스타일대로 구성되지 않고, 동일한 흐름을 갖게되어 다른 사람의 코드를 빠르게 파악할 수 있다. (협업에 유익)

<sup>(1)</sup> :컴포넌트 간 데이터 전달 방식은 아래에서 설명.

## 컴포넌트 간 데이터 전달 방법
### 상위(부모) 컴포넌트에서 하위(자식) 컴포넌트로 데이터 전달
<iframe height="265" style="width: 100%;" scrolling="no" title="props 속성을 사용한 데이터 전달 (상위 컴포넌트에서 하위 컴포넌트로)" src="//codepen.io/hongzoo/embed/eaKVXm/?height=265&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/hongzoo/pen/eaKVXm/'>props 속성을 사용한 데이터 전달 (상위 컴포넌트에서 하위 컴포넌트로)</a> by hong
  (<a href='https://codepen.io/hongzoo'>@hongzoo</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

### 하위(자식) 컴포넌트에서 상위(부모) 컴포넌트로 이벤트 전달

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="js,result" data-user="hongzoo" data-slug-hash="mYKxbL" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="이벤트를 발생시키고 수신하기 (하위 컴포넌트에서 발생 - 상위 컴포넌트에서 수신)">
  <span>See the Pen <a href="https://codepen.io/hongzoo/pen/mYKxbL/">
  이벤트를 발생시키고 수신하기 (하위 컴포넌트에서 발생 - 상위 컴포넌트에서 수신)</a> by hong (<a href="https://codepen.io/hongzoo">@hongzoo</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>