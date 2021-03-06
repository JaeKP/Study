# Vuex

[toc]

## 1. Vuex 기초

[공식문서](https://vuex.vuejs.org/#what-is-a-state-management-pattern)

상태관리 패턴 + 라이브러리

- 상태(state)를 전역 저장소로 관리할 수 있도록 지원하는 라이브러리이다.
  - 상태가 예측 가능한 방식으로만 변경될 수 있도록 보장하는 규칙을 설정한다.
  - 애플리케이션의 모든 컴포넌트에 대한 중앙 집중식 저장소 역할을 한다.
- Vue 공식 devtools와 통합되어 기타 고급 기능을 제공한다. 

<br>

### 1) 상태관리

#### (1) 상태관리 패턴

> State: data로 애플리케이션의 핵심이 되는 요소이다. 중앙에서 관리하는 모든 상태정보를 의미한다. 

-  컴포넌트의 공유된 상태를 추출하고 이를 전역에서 관리하도록 한다.
- 컴포넌트는 커다란 view가 되며 모든 컴포넌트는 트리에 상관없이 상태에 액세스 하거나 동작을 트리거 할 수 있다.
- 상태 관리 및 특정 규칙 적용과 관련된 개념을 정의하고 분리함으로써 코드의 구조와 유지 관리 기능이 향상된다.



`트리거`

특정한 동작에 반응해 자동으로 필요한 동작을 실행하는 것 

<br>

#### (2) 기존의 방식과의 차이점

| Pass Props & Emit Event                                      | Vuex                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 각 컴포넌트는 독립적으로 데이터를 관리한다.                  | 중앙 저장소(store)에 State를 모아놓고 관리한다.              |
| 데이터는 단방향 흐름으로 부모 -> 자식간의 전달만 가능하며 <br />반대의 경우에는 이벤트를 트리거한다. | 각 컴포넌트에서는 중앙 집중 저장소의 State만 신경 쓰면 된다. <br />(동일한 State를 공유하는 다른 컴포넌트들도 동기화가 된다. ) |
| 장점: 게이터의 흐름을 직관적으로 파악할 수 있다는 장점이 있다 | 장점: 그래서 규모가 큰 프로젝트에 매우 효율적이다.           |
| 단점: 컴포넌트 중첩이 깊어지는 경우 <br />동위 관계의 컴포넌트로의 데이터 전달이 불편해진다 |                                                              |

`단방향 데이터 흐름`

- State는 앱을 작동하는 원본 소스 (data)
- view는 state의 선언적 매핑
- action은 view에서 사용자 입력에 대해 반응적으로 state를 바꾸는 방법이다. (methods)

부모 자식간의 컴포넌트 관계가 단순하거나 depth가 깊지 않은 경우에는 큰 문제가 없다.  단계만 거치면 데이터를 쉽게 이동시킬 수 있으며 훨씬 직관적으로 데이터 흐름을 파악할 수 있다. 

하지만, 규모가 커졌을 경우 상태 관리가 어려워진다. 상태를 공유하는 컴포넌트의 상태 동기화 관리가 어렵다. (상태를 전달할 때 상=> 하로만 가능하다.)

그 결과, 상태의 변화에 따른 여러 프름을 모두 관리해야 하는 불편함을 해소할 필요가 생김. 상태는 데이터를 주고 받는 컴포넌트 사이의 관계도 충분히 고려해야 하기 때문에 상태 흐름 관리가 매우 중요해졌다.

결국, 이러한 상태를 올바르게 관리하는 저장소의 필요성을 느끼게 된다. 

-  상태를 한 곳(store)에 모두 모아 놓고 관리하자
- 상태의 변화는 모든 컴포넌트에서 공유한다.
- 상태의 변화는 오로지 Vuex가 관리하여 해당 상태를 공유하고 있는 모든 컴포넌트는 변화에 반응한다.

결과적으로, A 컴포넌트와 같은 상태를 공유하는 다른 컴포넌트는 신경쓰지 않고, 오로지 상태의 변화를 Vuex에 알린다. 

<br>

### 2) Vuex 구성요소

![](https://vuex.vuejs.org/vuex.png)

데이터 업데이트 (State)=> 화면이 바뀐다.(VueComonent) => 화면에서 행동을 한다. (Actions) => 데이터가 변경된다. (Mutations) => 데이터 업데이트 (State)



#### (1) State: 데이터

> 중앙에서 관리하는 모든 상태 정보 (data)

- Single state tree를 사용한다.
- 즉, 이 단일 객체는 모든 애플리 케이션 상태를 포함하는 원본 소스 (single source of truth)의 역할을 한다.
- 여러 컴포넌트 내부에 있는 특정 state를 중앙에서 관리하게 된다. 
  - 각 애플리케이션마다 하나의 저장소를 갖게 된다. (Vuex Store에서 각 컴포넌트에 해당하는 State 파악 가능)
  - 이전의 방식은 State를 찾기 위해 각 컴포넌트를 직접 확인해야 했다. 
  - 그러나, Vuex를 활요하는 방식은 Vuex Store에서 각 컴포넌트에서 사용하는 State를 한 눈에 파악이 가능하다.
- State가 변화하면 해당 State를 공유하는 여러 컴포넌트의 DOM은 (알아서) 렌더링된다.
- 각 컴포넌트는 이제 Vuex Store에서 State 정보를 가져와 사용한다.

<br>

#### (2) Mutations: single source of truth를 변경하는 작업

> 실제로 state를 변경하는 유일한 함수

- mutation의 handler함수는 반드시 동기적이여야한다.
  - 비동기적 로직은 state가 변화하는 시점이 의도한 것과 달라질 수 있으며, 콜백이 호출 될 시기를 알 수 있는 방법이 없다. (추적 불가능)
- 첫번째 인자로 항상 state를 받는다.
- **Actions에서 `commit()`메서드에 의해 호출된다.** 

<br>

#### (3)Actions

> 함수라는 점이 Mutations와 유사하지만 차이점이 있다. 

- context 객체 인자를 받는다. 
  - context 객체를 통해 store/index.js 파일 내의 있는 모든 요소의 속성 접근과 메서드 호출이 가능하다.

- 단, (가능하지만) State를 직접 변경하지 않는다. 
  - State를 변경하는 대신 Mutations를 commit()메서드를 호출해서 실행한다. 
- Mutations와 달리 비동기 작업이 포함될 수 있다. (API와 통신하여 DataFetching등의 작업을 수행한다.)
- **컴포넌트에서 `dispath()`메서드에 의해 호출된다.** 



#### (4) Getters

> State를 변경하지 않고 활용하여 계산을 수행한다. (computed 속성과 유사하다. )

- computed를 사용하는 것처럼 getters는 저장소의 상태(state)를 기준으로 계산한다.
- 예를 들어, state에 todoList라는 해야 할 일의 목록의 경우, 완료된 todo 목록만을 필터링해서 출력해야 하는 경우가 있다. 
- computed 속성과 마찬가지로 getters의 결과는 state 종속성에 따라 캐시(cached)되고, 종속성이 변경된 경우에만 다시 재계산 된다.
- getters 자체가 state를 변경하지는 않는다.
  - state를 특정한 조건에 따라 구분(계산)만 한다.
  - 즉, 계산된 값을 가져온다. 

<br>

## 2. 구현해보기

```bash
vue add vuex
```

- index.js 폴더가 생성된다.
  - index.js: Vuex core cocepts가 작성되는 곳

<br>

### 1) store

```javascript
// store/index.js

import Vue from "vue";
import Vuex from "vuex";
import createPersistedState from "vuex-persistedstate";

Vue.use(Vuex);

// 어플리케이션의 모든 Business Logic을 기록하는 공간!
export default new Vuex.Store({
  plugins: [createPersistedState()],
  // data
  state: {
    todos: [],
  },

  //computed
  getters: {
    allTodoCount(state) {
      return state.todos.length;
    },
    //현재 끝난 일의 개수
    completedTodoCount(state) {
      return state.todos.filter((todo) => {
        return todo.isCompleted;
      }).length;
    },
    unCompletedTodoCount(state) {
      return state.todos.filter((todo) => {
        return !todo.isCompleted;
      }).length;
    },
  },

  // methods => change
  // state를 변경하는 메소드를 작성한다. 함수 이름은 대문자 SNAKE_CASE를 사용한다.
  mutations: {
    CREATE_TODO(state, newTodo) {
      // 이렇게 하면 데이터는 변하지만 dom이 react하지 않는다!
      // state.todos[0] = newTodo
      state.todos.push(newTodo);
    },
    DELETE_TODO(state, deleteTodo) {
      const deleteTodoIndex = state.todos.indexOf(deleteTodo);
      state.todos.splice(deleteTodoIndex, 1);
    },
    UPDATE_TODO_STATUS(state, updateTodo) {
      state.todos = state.todos.map((todo) => {
        if (todo === updateTodo) {
          // ...todo, isCompleted: !todo.isCompleted
          todo.isCompleted = !todo.isCompleted;
        }
        return todo;
      });
    },
    LOAD_TODOS(state) {
      const stateString = localStorage.getItem("todos");
      state.todos = JSON.parse(stateString);
    },
  },

  // methods => !change
  // 변경에 필요한 준비작업(예 > 비동기 호출)을 수행한다.
  actions: {
    createTodo(context, newTodo) {
      context.commit("CREATE_TODO", newTodo);
    },
    deleteTodo({ commit }, deleteTodo) {
      commit("DELETE_TODO", deleteTodo);
    },
    updateTodoStatus({ commit }, updateTodo) {
      commit("UPDATE_TODO_STATUS", updateTodo);
    },
  },
  modules: {},
});

```

- Actions의 `context`객체

  - Vuex store의 전반적인 맥락 속성을 모두 포함하고 있다.
  - 그래서 context.commit을 호출하여 mutation을 호출하거나,
    - context.state와 context.getters를 통해 state와 getters에 접근 할 수 있다.
    - dispatch()로 다른 actions도 호출이 가능하다.
  - 할 수 있지만 actions에서 state를 조작하면 안된다.

- Vuex-persistedstate

  - Vuex sate가 자동으로 브라우저의 LocalStorage에 저장해주는 라이브러리 중 하나이다.

  - 페이지가 새로고침 되어도 Vuex state를 유지시킨다.

    ```bash
    npm i vuex-persistedstate
    ```

<br>

### 2. App.vue

```vue
<template>
  <div id="app">
    <h1>Todo</h1>
    <h3>전체: {{ allTodoCount }}</h3>
    <h3>완료: {{ $store.getters.completedTodoCount }}</h3>
    <h3>진행: {{ unCompletedTodoCount }}</h3>
    <todo-list></todo-list>
    <todo-form></todo-form>
  </div>
</template>

<script>
import TodoList from "@/components/TodoList.vue";
import TodoForm from "@/components/TodoForm.vue";
import { mapGetters, mapMutations } from "vuex";
export default {
  name: "App",
  components: {
    TodoList,
    TodoForm,
  },
  computed: {
    ...mapGetters([
      "allTodoCount",
      "completedTodoCount",
      "unCompletedTodoCount",
    ]),
  },
  methods: {
    ...mapMutations(["LOAD_TODOS"]),
  },
};
</script>

<style scoped>
div {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>

```

- **`...object` Javascript Spread Syntax**
  - 전개 구문
  - 배열이나 문자열과 같은 반복 가능한(iterable)문자를 요소(배열 리터럴의 경우)로 확장하여, 0개 이상의 key-value의 쌍으로 된 객체를 확장 시킬 수 있다.
  - `...`을 붙여서 요소 또는 키가 0개 이상의 iterable object를 하나의 object로 간단하게 표현하는 법
  - ECMAScript 2015에서 추가 된다.
  - Spread Syntax의 대상은 반드시 iterable 객체여야 한다. 
- **Component Binding Helper** 
  - `mapState()`
    - computed와 Store의 state를 매핑한다.
    - Vuex Store의 하위 구조를 반환하여 component 옵션을 생성한다.
    - 매핑된 computed 이름이 state 이름과 같을 때 문자열 배열을 전달 할 수 있다. 
    - 하지만 다른 computed 값을 함께 사용할 수 없기 때문에 최종 객체를 computed에 전달할 수 있도록 객체 전개 연산자로 객체를 복사하여 작성한다.
    - mapState()는 객체를 반환한다.
  - `mapGetters`
    - computed와 Getters를 매핑한다.
    - getters를 객체 전개 연산자로 계산하여 추가한다.
    - 해당 컴포넌트 내에서 매핑하고자 하는 이름이 index.js에 정의해 놓은 getters의 이름과 동일하면 배열의 형태로 해당 이름만 문자열로 추가한다. 
  - `mapActions`
    - action을 전달하는 컴포넌트 method 옵션을 만든다.
    - actions를 객체 전개 연산자로 계산하여 추가한다.
    - mapActions를 사용하면, 이전에 dispatch()를 사용했을 때, playload로 넘겨줬던 this.todo를 pass pop로 변경해서 전달해야 한다. 

<br>

### 3) Components

```vue
<!--src/components/TodoForm.vue --> 

<template>
  <div>
    <input type="text" @keyup.enter="createTodo" v-model.trim="todoTitle" />
  </div>
</template>

<script>
export default {
  name: "TodoForm",
  data() {
    return {
      todoTitle: "",
    };
  },
  methods: {
    createTodo() {
      const newTodo = {
        title: this.todoTitle,
        isCompleted: false,
        date: new Date().getTime(),
      };
      // 2번째 인자가 넘겨줄 데이터!
      this.$store.dispatch("createTodo", newTodo);
      this.todoTitle = "";
    },
  },
};
</script>

<style></style>

```

<br>

```vue
<!--src/components/TodoList.vue -->

<template>
  <div>
    <todo-list-item
      v-for="todo in todos"
      :key="todo.date"
      :todo="todo"
    ></todo-list-item>
  </div>
</template>

<script>
import TodoListItem from "@/components/TodoListItem.vue";

export default {
  name: "TodoList",
  components: {
    TodoListItem,
  },
  computed: {
    todos() {
      return this.$store.state.todos;
    },
  },
};
</script>

<style></style>
```

<br>

```vue
<!-- src/components/TodoListItem-->

<template>
  <div>
    <!-- 인자를 추가로 전달해야 할때는 ()안에 넣어서 보내준다. -->
    <span
      @click="updateTodoStatus(todo)"
      :class="{ 'is-complted': todo.isCompleted }"
    >
      {{ todo.title }}
    </span>
    <button @click="deleteTodo(todo)">X</button>
  </div>
</template>

<script>
import { mapActions } from "vuex";

export default {
  name: "TodoListItem",
  props: {
    todo: {
      type: Object,
    },
  },
  methods: {
  // 3. ...sytex 사용 
    ...mapActions(["deleteTodo", "updateTodoStatus"]),
  },
  // 2. actions의 'deleteTodo'함수를 바로 쓰고 싶다! => 바인딩 헬퍼
  //deleteTodo =  vuex.mapActions(['deleteTodo', 'createTodo']).deleteTodo
  //deleteTodo =  mapActions(['deleteTodo', 'createTodo']).deleteTodo

  // 1. store에 삭제 요청하기!
  // deleteTodo() {this.$store.dispatch("deleteTodo", this.todo);},
};
</script>

<style scoped>
.is-complted {
  text-decoration: line-through;
}

div {
  border: 2px solid black;
  margin: 2px;
  padding: 2px;
}
</style>

```

