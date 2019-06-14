# 뷰 라우터

## 라우팅이란?
* 웹 페이지 간의 이동 방법
* 싱글 페이지 애플리케이션(SPA)에서 주로 사용
* 브라우저에서 웹 페이지를 요청할 때 라우팅으로 처리하면 깜빡거림 없이 화면을 매끄럽게 전환 & 빠르게 화면 조작 가능
  > 일반적으로는 서버에서 응답을 받아 웹 페이지를 다시 사용자에게 돌려주는 시간 동안 화면 상에 깜빡거림 현상이 나타남

## 뷰 라우터 특수 태그
```html
<router-link to="URL 값">
```
> 페이지 이동 태그: 화면에서는 `<a>`로 표시되며 클릭하면 `to=""`에 지정한 URL로 이동

```html
<router-view>
```
> 페이지 표시 태그: 변경되는 URL에 따라 해당 컴포넌트를 뿌려주는 영역

### 뷰 라우터 실습
실습파일: [codeview](exercise01.html) / [pageview](https://hongzoo.github.io/Vue.js/mytest/04-1/exercise01.html)  
See the Pen: <a href="https://codepen.io/hongzoo/pen/MMYdGp/">
  뷰 라우터 실습</a> by hong (<a href="https://codepen.io/hongzoo">@hongzoo</a>)
  on <a href="https://codepen.io">CodePen</a>.
> 코드펜에서도 실습했지만,  
> 라우터 실습은 주소창에 링크가 바뀌는 걸 확인해야 하므로 실습파일로 확인하는 것이 좋음!

<!-- 테스트결과가 좀 이상해서 일단 보류.. -->
<!-- #### 라우터 URL의 해시 값(#)을 없애는 방법
실습파일 [pageview](https://hongzoo.github.io/Vue.js/mytest/04-1/exercise01.html)에서 주소창을 확인해보면 #이 표시되어있다.
```
exercise01.html#
exercise01.html#/main
exercise01.html#/login
```
뷰 라우터의 기본 URL 형식은 해시 값을 사용하기 때문에 `exercise01.html/main` 처럼 해시 값을 없애려면 히스토리 모드를 활용하면 된다!
```javascript
var router = new VueRouter({
  mode: 'history',
  routes
});
```
> 히스토리 모드를 적용한 실습파일 [pageview](https://hongzoo.github.io/Vue.js/mytest/04-1/exercise01_historyMode.html) -->

## 네스티드 라우터 (Nested Router)
> 상위 컴포넌트 1개에 하위 컴포넌트 1개를 포함하는 구조  
> 라우터로 페이지를 이동할 때 최소 2개 이상의 컴포넌트를 화면에 나타낼 수 있음

실습파일: [codeview](exercise02.html) / [pageview](https://hongzoo.github.io/Vue.js/mytest/04-1/exercise02.html)  
> 1. 페이지 열어서 주소창 URL값의 끝에 `user` 입력 해보기
> 2. 주소창 URL값의 끝에 `/post` 추가 입력 -> UserPost 컴포넌트가 나타나는지 확인
> 3. 주소창 URL값의 끝에 `/profile` 추가 입력 -> UserProfile 컴포넌트가 나타나는지 확인

**네스티드 라우터의 한계점**
> 화면을 구성하는 컴포넌트의 수가 적을 때는 유용하지만 한 번에 더 많은 컴포넌트를 표시하는데는 한계가 있다.  
> 이를 해결할 수 있는 방안으로 `네임드 뷰`가 있다.

## 네임드 뷰 (Named View)
> 같은 레벨에서 여러 개의 컴포넌트를 한 번에 표시.  
> 특정 페이지로 이동했을 때 여러 개의 컴포넌트를 동시에 표시하는 라우팅 방식

실습파일: [codeview](exercise03.html) / [pageview](https://hongzoo.github.io/Vue.js/mytest/04-1/exercise03.html)  

**네스티드 라우터와 네임드 뷰 라우터 작성부분 차이점**
```js
routes: [
  path: '/user',
  component: User,
  children: [
    {
      path: 'posts',
      components: UserPost
    },
    {
      path: 'profile',
      components: UserProfile
    }
  ]
]
```
> 네스티드 라우터: children 객체 키로 하위 라우터를 정의한다

```js
routes: [
  {
    path: '/',
    components: {
      default: Body,
      header: Header,
      footer: Footer
    }
  },
  {
    path: '/child',
    components: {
      default: ChildBody,
      header: Header,
      footer: Footer
    }
  }
]
```
> 네임드뷰 라우터: component키의 값에 `<router-view name="">` name 속성값을 나열하여 정의한다. 하위 라우터는 `path: '/child` 가 있는 객체처럼 또 하나 추가한다.