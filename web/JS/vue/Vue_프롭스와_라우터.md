# Vue 프롭스와 라우터

[toc]

## 1. SFC (Single File Component)

- Vue의 컴포넌트 기반 개발의 핵심 특징이다. 
- 하나의 컴포넌트는 `.vue`확장자를 가진 하나의 파일 안에서 작성되는 코드의 결과물이다.
- 화면의 특정 영역에 대한 HTML, CSS, JavaScript 코드를 하나의 파일(`.vue`)에서 관리한다.
- 즉, `.vue` 확장자를 가진 싱글 파일 컴포넌트를 통해 개발하는 방식이다. 

<br>

### 1) Component

[공식문서](https://kr.vuejs.org/v2/guide/components.html#%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EA%B0%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94)

> 기본 HTML 엘리먼트를 확장하여 재 사용 가능한 코드를 캡슐화 하는데 도움을 준다.

- CS에서는 다시 사용할 수 있는 범용성을 위해 개발된 소프트웨어 구성 요소를 의미한다.
- 컴포넌트는 유지보수를 쉽게 만들어 줄 뿐만 아니라, 재사용성의 측면에서도 매우 강력한 기능을 제공한다. 

<br>

| 단일 파일에서의 개발                                         | 각 기능 별로 파일을 나눠서 개발                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 처음 개발을 시작 할 때는 크게 신경 쓸 것이 없기 때문에 쉽게 개발이 가능하다. | 처음 개발을 준비하는 단계에서 시간 소요가 증가한다.          |
| 하지만 코드의 양이 많아지면 변수 관리가 힘들어지고 유지보수에 많은 비용이 발생한다. | 하지만 이후 변수 관리가 용이하며 기능 별로 유지 & 보수 비용이 감소한다. |

- 한 화면 안에서도 기능 별로 각기 다른 컴포넌트가 존재한다. 

- 하나의 컴포넌트는 여러 개의 하위 컴포넌트를 가질 수 있다. Vue는 컴포넌트 기반의 개발 환경을 제공한다. 
- 컴포넌트 기반의 개발이 반드시 파일 단위로 구분되어야 하는 것은 아니다. 
  - 단일 `.html`파일 안에서도 여러 개의 컴포넌트를 만들어 개발이 가능하다. 

<br>

### 2) Component의 데이터는 반드시 함수여야 한다. 

[공식문서](https://kr.vuejs.org/v2/guide/components.html#data-%EB%8A%94-%EB%B0%98%EB%93%9C%EC%8B%9C-%ED%95%A8%EC%88%98%EC%97%AC%EC%95%BC%ED%95%A9%EB%8B%88%EB%8B%A4)

```vue
data: function () {
  return {
    counter: 0
  }
}
```

기본적으로 각 인스턴스는 모든두 같은 data객체를 공유하므로, 새로운 data객체를 반환해야 한다. 

 <br>

## 2. Vue CLI

### 1) Node.js

- 자바스크립트를 브라우저가 아닌 환경에서도 구동할 수 있도록 하는 자바스크립트 런타임 환경이다. 
  - 브라우저 밖을 벗어 날 수 없던 자바스크립트 언어의 태생적 한계를 해결했다.
- Chrome V8 엔진을 제공하여 여러 OS 환경에서 실행할 수 있는 환경을 제공한다.
- 즉, 단순히 브라우저만 조작할 수 있던 자바스크립트를 SSR 아키텍처에서도 사용할 수 있도록 했다.

<br>

### 2) NPM (Node Package Manage)

- 자바스크립트 언어를 위한 패키지 관리자
  - python에는 pip가 있다면 Node.js에는 NPM이 있다.
  - pip와 마찬가지로 다양한 의존성 패키지를 관리한다.
- Node.js의 기본 패키지 관리자
- Node.js 설치 시 함께 설치됨

<br>

```bas
npm install -g @vue/cli
vue--version

Vue create my-first-app
cd my-first-app
npm run serve
```

<br>

## 3. Babel & Webpack

### 1) Babel

> JavaScript Compiler

- 자바스크립트의 ECMAScript 2015+ 코드를 이전 버전으로 번역 / 변환 해주는 도구
- 과거 자바스크립트의 파편화와 표준화의 영향으로 코드의 스펙트럼이 매우 다양하다.
  - 이 때문에 최신 문법을 사용해도 이전 브라우저 혹은 환경에서 동작하지 않는 상황이 발생했다.
- 원시 코드(최신 버전)를 목적 코드(구 버전)로 옮기는 번역기가 등장하면서 개발자는 더 이상 내 코드가 특정 브라우저에서 동작하지 않는 상황에 대해 크게 고민하지 않을 수 있게 됨 

<br>

### 2) Webpack

> static module bundler

- 모듈 간의 의존성 문제를 해결하기 위한 도구
- 프로젝트에 필요한 모든 모듈을 매핑하고 내부적으로 종속성 그래프를 빌드한다. 
- 모듈은 단지 파일 하나를 의미한다. ex) js 파일 하나 === 모듈 하나
  - 브라우저만 조작할 수 있던 시기의 자바스크립트는 모듈 관련 문법 없이 사용되었다.
  - 하지만 JS와 애플리케이션이 복잡해지고 크기가 커지자 전역 scope를 공유하는 형태의 기존 개발 방식의 하계점이 드러남
  - 그래서 라이브러리를 만들어 필요한 모듈을 언제든지 불러오거나 코드를 모듈 단위로 작성하는 등의 다양한 시도가 이루어진다. 



`여러 모듈 시스템`

- ESM (ECMA Script Module)
- AMD (Asynchoronous Module Definition)
- CommonJS
- UMD (Universal Module Definition)



`모듈의 의존성 문제`

- 모듈의 수가 많아지고 라이브러리 혹은 모듈 간의 의존성이 깊어지면서 특정한 곳에서 발생한 문제가 어떤 모듈 간의 문제인지 파악하기 어렵다.
- 즉, Webpack은 이 모듈 간의 의존성 문제를 해결하기 위해 등장했다. 



`Static Module Bundler`

- 모듈 의존성 문제를 해결해주는 작업을 Bundling이라 한다.
- 이러한 일을 해주는 도구가 Bundler이고, Webpack은 다양한 Bundler 중 하아니다.
- 여러 모듈을 하나로 묶어주고 묶인 파일은 하나 (혹은 여러 개)로 합쳐진다.
- Bundling된 결과물은 더 이상 순서에 영향을 받지 않고 동작하게 된다.
- snowpack, parcel, rollup.js 등의 webpack 이외에도 다양한 모듈 번들러가 존재한다.

Vue CLI는 이러한 Bable, Webpack에 대한 초기 설정이 자동으로 되어 있음

<br>

| Node.js                                                 | Babel                                                        | Webpack                                   |
| ------------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------- |
| JavaScript Funtime Environment                          | Compiler                                                     | Module Bundler                            |
| JavaScript를 브라우저 밖에서 실행할 수 있는 새로운 환경 | ES2015 + JavaScript 코드를 구 버전의 JavaScript로 바꿔준느 도구 | 모듈간의 의존성 문제를 해결하기 위한 도구 |

<br>

### 3) Vue 프로젝트 구조

| 파일 이름           | 설명                                                         |
| ------------------- | ------------------------------------------------------------ |
| `node_modules`      | node.js 환경의 여러 의존성 모듈                              |
| `public/index.html` | Vue 앱의 뼈대가 되는 파일<br />실제 제공되는 단일 html 파일  |
| `src/assets`        | webpack에 의해 빌드 된 정적 파일                             |
| `src/components`    | 하위 컴포넌트들이 위치                                       |
| `src/App.vue`       | 최상위 컴포넌트                                              |
| `src/main.js`       | webpack이 빌드를 시작할 때 가장 먼저 불러오는 entty.point<br />실제 단일 파일에서 DOM과 data를 연결 했던 것과 동일한 작업이 이루어지는 곳<br />Vue 전역에서 활용 할 모듈을 등록할 수 있는 파일 |
| `babel.config.js`   | babel 관련 설정이 작성된 파일                                |
| `package.json`      | 프로젝트 종속성 목록과 지원되는 브라우저에 대한 구성 옵션이 포함된다. |
| `package-lock.json` | node_modules에 설치되는 모듈과 관련된 모든 의존성을 설정 및 관리<br />팀원 및 배포 환경에서 정확히 동일한 종속성을 설치하도록 보장하는 표현<br />사용할 패키지의 버전을 고정<br />개발 과정간의 의존성 패키지 충돌 방지 |

<br>

## 4. Props와 emit 

> Vue app은 자연스럽게 중첩된 컴포넌트 트리로 구성된다. 
>
> 컴포넌트간 부모 -자식 관계가 구성되어 이들간의 통신이 필요하다. 

- 템플릿 (HTML)
  - HTML의 body 부분
  - 각 컴포넌트를 작성한다. 

- 스크립트 (JavaScript)
  - JS가 작성되는 곳
  - 컴포넌트 정보, 데이터, 메서드 등 vue 인스턴스르르 구성하는 대부분이 작성된다. 

- 스타일 (CSS)
  - CSS가 작성되며 컴포넌트의 스타일을 담당한다. 




`자식 컴포넌트 등록 3단계`

```vue
<!-- App.vue -->

<template>
  <div id="app">
    <!-- 3. 보여주기 (print): 카멜 케이스 혹은 케밥 케이스로 작성 -->
    <theAbout/>
    <the-about></the-about>
  </div>
</template>

<script>
// 1. 불러오기 (import)
import TheAbout from "./components/TheAbout.vue";

export default {
  name: "App",
  // 2. 등록하기 (register)
  components: {
    // The About: TheAbout
    TheAbout,
  },
};
</script>

<style></style>

```



### 1) props



<img src="https://kr.vuejs.org/images/props-events.png" style="zoom:50%;" />



- 부모는 자식에게 데이터를 전달 (Pass props)하며, 자식은 자신에게 일어난 일을 부모에게 알린다. (Emit event)

- 부모와 자식이 명확하게 정의된 인터페이스를 통해 격리된 상태를 유지할 수 있음 

-  부모는 `props`를 통해 자식에게 데이터를 전달하고, 자식은 `event`를 통해 부모에게 메시지를 보냄



#### (1) props

- 부모 컴포넌트의 정보를 자식 컴포넌트에게 전달하기위한 사용자 지정 특성이다.

- 자식 컴포넌트는 props 옵션을 사용하여 수신하는 props를 명시적으로 선언해야 한다.

- 모든 컴포넌트 인스턴스에는 자체 격리된 범위가 있기 때문에 자식 컴포넌트의 템플릿에서 상위 데이터를 직접 참조할 수 없다.  

  

#### (2) 구현

- during declaration(선언 시): 카멜 케이스
- intemplate(HTML): 케밥 케이스

```react
<!-- App.vue -->

<template>
  <div id="app">
    <!-- bind를 사용하면 부모의 데이터를 동적으로 바인딩한다. (데이터변화가 있으면 자식데이터로 전달된다)-->
    <!-- bind를 사용하지 않으면 무조건 문자열로 전달! -->
    <theAbout my-message="parentData"/>
    <the-about :my-message="parentData"></the-about>
  </div>
</template>

<script>
import TheAbout from "./components/TheAbout.vue";

export default {
  name: "App",
  components: {
    TheAbout,
  },
  data: function () {
    return { parentData: "This is parent data to child components" };
  },
};
</script>

<style></style>

```

<br>

```vue
<!-- components/TheAbout.vue -->

<template>
  <div>
    <h1>{{ myMessage }}</h1>
  </div>
</template>

<script>
export default {
  name: "TheAbout",
  props: {
    myMessage: {
        type: String
    }
  },
};
</script>

<style></style>
```

<br>

#### (3) 단방향 데이터 흐름

- 모든 props는 하위 속성과 상위 속성 사이의 단방향 바인딩을 형성한다. 
- 부모의 속성이 변경되면 자식 속성에게 전달되지만, 반대 방향으로는 안된다. 
  - 자식 요소가 의도치 않게 부모 요소의 상태를 변경하여 앱의 데이터 흐름을 이해하기 어렵게 만드는 일을 방지한다.
- 부모 컴포넌트가 업데이트 될 때마다 자식 요소의 모든 pop들이 최신 값으로 업데이트 된다. 

<br>

### 2) $emit

[공식문서](https://kr.vuejs.org/v2/guide/components-custom-events.html)

> 현재의 인스턴스에서 이벤트를 트리거하는 역할을 한다. 추가 인자는 리스터의 콜백 함수로 전달된다. 

- 부모 컴포넌트는 자식 컴포넌트가 사용되는 템플릿에서 v-on을 사용하여 자식 컴포넌트가 보낸 이벤트를 청취한다. (v-on을 이용한 사용자 지정 이벤트)
- 자식 컴포넌트에서 $emit 인스턴스 메서드를 통해 특정 이벤트를 트리거한다. 
- 부모 컴포넌트는 자식컴포넌트가 트리거한 이벤트를 v-on디렉티브를 사용하여 듣는다. 

<br>

#### (1) 구현

- 컴포넌트 및 props와 달리, 이벤트는 자동 대소문자 변환을 제공하지 않기 때문에 이벤트 이름으로 항상 케밥케이스를 사용하는 것을 권장한다. 
  - @myEvent => @myevent가 된다.
  - @my-event사용을 권장한다. 

<br>

```vue
<!-- components/TheAbout.vue -->

<template>
  <!-- template 안에는 반드시 하나의 Element가 들어갈 수 있다.  -->
  <div>
    <h1>{{ myMessage }}</h1>
    <input
      type="text"
      v-model="childInputData"
      @keyup.enter="childInputChange"
    />
  </div>
</template>

<script>
export default {
  name: "TheAbout",
  props: {
    myMessage: String,
  },
  data: function () {
    return {
      childInputData: "",
    };
  },
  methods: {
    childInputChange: function () {
      // 부모 컴포넌트에게 child-input-change는 이름의 이벤트를 발생시킨다.
      // (생성한 이벤트 이름 , 보내는 데이터)
      // 만약 보내는 데이터가 여러개라면 객체로 묶어서 보낸다. 
      this.$emit("child-input-change", this.childInputData);
    },
  },
};
</script>

<style></style>

```

<br>

```vue
<!-- App.vue -->

<template>
  <div id="app">
    <!-- 자식이 뭐하는지 귀를 달아줘요.. -->
    <the-about
      :my-message="parentData"
      @child-input-change="parentGetChange"
    ></the-about>
  </div>
</template>

<script>
import TheAbout from "./components/TheAbout.vue";

export default {
  name: "App",
  components: {
    TheAbout,
  },
  data: function () {
    return { parentData: "This is parent data to child components" };
  },
   // 자식이 보내는 데이터가 매개변수로 넘어온다. 
  methods: {
    parentGetChange: function (inputData) {
      console.log(inputData);
    },
  },
};
</script>

<style></style>

```

<br>

## 5. router

[공식문서](https://router.vuejs.org/guide/)

Creating a Single-page Application with Vue + Vue Router feels natural: with Vue.js, we are already composing our application with components. When adding Vue Router to the mix, all we need to do is map our components to the routes and let Vue Router know where to render them. 

즉, 컴포넌트를 라우터에 매칭하고 vue router에게 랜더링할 위치를 알려주면 된다.여기서 라우터란 위치에 대한 최적의 경로를 지정하며, 이 경로에 따라 데이터를 다음 장치로 전향시키는 장치를 의미한다. 

<br>

```vue
<!-- 기존 프로젝트를 진행하고 있던 도중에 추가하게 되면 App.vue를 덮어쓰므로 
프로젝트 내에서 다음 명령을 실행하기 전에 필요한 경우 파일을 백업해야 한다. --> 
vue add router
```



`history mode`

- HTML History API를 사용해서 router를 구현한 것이다.
- 브라우저의 히스토리는 남기지만 실제 페이지는 이동하지 않는 기능을 지원한다.
- 즉, 페이지를 다시 로드하지 않고 URL을 탐색할 수 있다.
  - SPA 단점 중 하나인 "URL이 변경되지 않는다"를 해결한다.
- DOM window 객체는 history 객체를 통해 브라우저의 세션 기록에 접근할 수 있는 방법을 제공한다.
- history 객체는 사용자를 자신의 방문 기록 앞 뒤로 보내거나, 기록의 특정 지점으로 이동하는 등 유용한 메서드와 속성을 가진다. 

<br>

`프로젝트 구조`

- 기본적으로 작성된 구조에서 components 폴더와 views 폴더 내부에 각기 다른 컴포넌트가 존재하게 된다.
- 컴포넌트를 작성해 갈 때 정해진 구조가 있는 것은 아니며, 주로 아래와 같이 구조화하여 활용한다.

| 이름        | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| App.vue     | 최상위 컴포넌트                                              |
| views/      | router(index.js)에 매핑되는 컴포넌트를 모아두는 폴더         |
| components/ | router에 매핑된 컴포넌트 내부에 작성하는 컴포넌트를 모아두는 폴더 |

<br>

`Vue Router가 필요한 이유`

- SPA 등장 이전: 서버가 모든 라우팅을 통제하고 요청 경로에 맞는 HTML을 제공했다.
- SPA 등장 이후: 서버는 index.html 하나만 제공하고 이후 모든 처리는 HTMl 위에서 JS 코드를 활용해 진행했다.
  - 즉, 요청에 대한 처리를 더 이상 서버가 하지 않는다. (할 필요가 없어졌다.)

SSR: 라우팅에 대한 결정권을 서버가 가진다.

CSR: 클라이언트는 더 이상 서버로 요청을 보내지 않고 응답받은 HTML 문서 안에서 주소가 변경되면 특정 주소에 맞는 컴포넌트를 렌더링한다.
라우팅에 대한 결정권을 클라이언트가 가진다.

결국 Vue Router는 라우팅 결정권을 가진 Vue.js에서 라우팅을 편리하게 할 수 있는 Tool을 제공해주는 라이브러리이다. 

<br>

### 1) index.js

- router 폴더에 index.js가 생성된다. 
- index.js: 라우터에 관련된 정보 및 설정이 작성되는 곳이다. 

```javascript
<!--router/ index.js --> 

import Vue from "vue";
import VueRouter from "vue-router";
import HomeView from "../views/HomeView.vue";
import AboutView from "../views/AboutView.vue";
import UserProfile from "../views/UserProfile.vue";

Vue.use(VueRouter);

const routes = [
  {
    path: "/",
    name: "home",
    component: HomeView,
  },
  {
    path: "/about",
    name: "about",
    component: AboutView,
  },
 
  /* 동적 인자를 전달한다. (주어진 패턴을 가진 라우트를 동일한 컴포넌트에 매핑해야 하는 경우)
  동적인자는 :(콜론)으로 시작한다. 컴포넌트에서는 this.$route.params로 사용이 가능하다.*/
  {
    path: "/user/:userId",
    name: "profile",
    component: UserProfile,
  },
];

const router = new VueRouter({
  mode: "history",
  base: process.env.BASE_URL,
  routes,
});

export default router;
```

<br>

```vue
<!-- views/ UserProfile.vue --> 

<template>
  <div>
    <h1>UserProfile</h1>
    <p>당신의 id는 {{ user.userId }}</p>
  </div>
</template>

<script>
export default {
  name: "UserProfile",
  data: function () {
    return {
      user: this.$route.params,
    };
  },
};
</script>

<style></style>

```

<br>

| pattern                            | matched path       | $route.params                 |
| ---------------------------------- | ------------------ | ----------------------------- |
| /user/:userName                    | /user/jk           | {username: 'jk'}              |
| /user/:userName/article/:articleId | /user/jk/article/1 | {username:'jk', articleId: 1} |

<br>

### 2) App.vue

```react
<!-- App.vue --> 

<template>
  <div id="app">
    <nav>
      <!-- 새로 고침을 발생하지 않는다. url로 이동하는 척!한다. -->
      <!-- 실제 화면 전환이 발생하는 것이 아니다. 눈속임! -->

      <!-- <router-link to="/">Home</router-link> | -->
      <router-link :to="{ name: 'home' }">Home</router-link> |
      <router-link :to="{ name: 'about' }">About</router-link>
      <router-link :to="{ name: 'profile', params: {userId: 1} }">About</router-link>
    </nav>
    <!-- 실제로 이자리가 대체된다.  -->
    <!-- AboutView -->
    <!-- HomeView -->
    <router-view />
  </div>
</template>

<style></style>

```

- `<router-link>`
  - 사용자의 네비게이션을 가능하게 하는 컴포넌트이다. 
  - 목표 경로는 `to` prop으로 지정된다.
  - HTML5 히스토리 모드에서 router-link는 클릭 이벤트를 차단하여 브라우저가 페이지를 다시 로드 하지 않도록 한다.
  - a태그지만 우리가 알고 있는 GET 요청을 보내는 a 태그와 조금 다르게, 기본 GET 요청을 보내는 이벤트를 제거한 형태로 구성된다. 
- `<router-view>`
  - 주어진 라우트에 대해 일치하는 컴포넌트를 렌더링하는 컴포넌트이다.
  - 실제 componenet가 DOM에 부착되어 보이는 자리를 의미한다.
  - router-link를 클릭하면 해당 경로와 연결되어 있는 index.js에 정의한 컴포넌트가 위치한다. 

<br>

### 3) 프로그래밍 방식의 네비게이션

> <router-link>를 사용하여 선언적 탐색을 위한 a 태그를 만드는 것 외에도
>
> router의 인스턴스 메서드를 사용하여 프로그래밍 방식으로 같은 작업을 수행할 수 있다.

<br>

```vue
<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click="moveToHome">To home</button>
  </div>
</template>

<script>
export default {
  name: "AboutView",
  methods: {
    moveToHome() {
      this.$router.push({ name: "home" });
    },
  },
};
</script>

<style></style>
```

<br>

| 선언적 방식            | 프로그래밍 방식     |
| ---------------------- | ------------------- |
| `<router-link to ="">` | `$router.push(...)` |

- Vue 인스턴스 내부에서 라우터 인스턴스에 `$router`로 접근할 수 있다.

- 따라서 다른 URL로 이동하려면 `this.$router.push`를 호출할 수 있다.

  - 이 메서드는 새로운 항목을 히스토르 스택에 넣기 때문에 사용자가 브라우저의 뒤로 가기 버튼을 클릭하면 이전 URL로 이동하게 된다.
  - `<router-link>`는 클릭할 때 내부적으로 호출되는 메서드이므로 `<router-link : to=""`를 클릭하면, `router.push(...)`를 호출하는 것과 같다. 

- 작성할 수 있는 인자 예시

  ```javascript
  // 문자열 path 
  router.push('home')
  
  // object
  router.push({path: 'home'})
  
  // name 라우트
  router.push({name: 'user', params: {userId: '123'}})
  
  // 쿼리=>  /register?plan=private
  router.push({path: 'register', query:{plan: 'private'}})
  ```

- params가 있을 경우에는 `this.$route.param`로 사용이 가능하다. 



<br>
