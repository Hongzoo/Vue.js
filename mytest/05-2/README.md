# 싱글 파일 컴포넌트 체계

**HTML 파일에서 뷰 코드 작성 시의 한계점**  
앞쪽에서 다뤘던 예제나 실습은 모두 HTML 파일에서 자바스크립트와 마크업을 이용했는데, 실제로 애플리케이션을 제작하다 보면 이런 파일구조에서 한계점을 느끼게 된다.

`<script>`태그 안에서 템플릿을 정의할 때 HTML 코드는 구문 강조가 적용되지 않기 때문에 오탈자를 찾기 어렵다. 또한 코드 들여쓰기도 어려워 태그 구조를 파악하기도 어렵다.

이러한 문제점을 해결하는 방법이 '싱글 파일 컴포넌트` 체계이다.

## .vue 파일
싱글 파일 컴포넌트 체계란 .vue 파일로 프로젝트 구조를 구성하는 방식. 확장자 .vue 파일 1개는 뷰 애플리케이션을 구성하는 1개의 컴포넌트와 동일함.

```v
<template>
  <!-- HTML 태그 내용 (s) -->
  <div>
    <span>
     <button>{{ message }}</button>
    </span>
  </div>
  <!-- HTML 태그 내용 (e) -->
</template>

<script>
export default{
  // 자바스크립트 내용 (s)
  data: {
    message: 'click this button'
  }
  // 자바스크립트 내용 (e)
}
</script>

<style>
  /* CSS 스타일 내용 (s) */
  span {
    font-size: 1.2em;
  }
  /* CSS 스타일 내용 (e) */
</style>
```
> .vue 파일의 기본 구조

**여기서 잠깐! `export default{}`?**  
ES6의 자바스크립트 모듈화와 관련된 문법. 자바스크립트는 변수의 유효 범위가 파일 단위로 구분되지 않기 때문에 기존에 정의된 변수를 실수로 재정의하거나 유효 범위가 충돌할 경우를 방지하기 위해 모듈화를 한다.

## .vue 파일을 웹 브라우저가 인식할 수 있도록 필요한 모듈 번들러 (feat. 뷰 CLI)

싱글 파일 컴포넌트 체계를 사용하기 위해서는 .vue 파일을 웹 브라우저가 인식할 수 있는 형태의 파일로 변환이 필요하다. 아래 같은 모듈 번들러 도구가 필요.
- 웹팩(Webpack)
- 브라우저리파이(Browserify)

뷰 개발자들이 편하게 저 도구들을 사용해서 프로젝트를 구성할 수 있도록 CLI 도구가 제공된다. (CLI: 커맨드 창에서 명령어로 특정 동작을 수행할 수 있는 도구)