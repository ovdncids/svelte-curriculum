# Svelte (5.38.0)
* [데모](https://curriculums-min.web.app)

## Node.js
* https://nodejs.org

## NVM (Node Version Manager)
* https://github.com/ovdncids/react-curriculum/blob/master/NVM.md

## SvelteKit vs Svelte
* [Svelte](https://svelte.dev/docs/svelte/overview)는 React, Vue.js의 기본 문법과 같이 `UI 컴포넌트를 렌더링하는 프레임워크`에 필요한 기본 문법을 뜻함.
* [SvelteKit](https://svelte.dev/docs/kit/introduction)은 로컬 서버, 테스트, 빌드, 개발 환경등을 설정 가능한 `웹 애플리케이션`을 만들 수 있도록 도와주는 도구를 뜻함.

## Svelte CLI
* https://svelte.dev/docs/kit/creating-a-project
```sh
# Svelte 프로젝트 생성
npx sv create svelte-study
# Which template would you like? SvelteKit minimal
# Add type checking with TypeScript? Yes, using TypeScript syntax
# What would you like to add to your project? (추후에 추가 가능. npx sv add tailwindcss)

cd svelte-study
code .
npm run dev
# or
npm run dev -- --open 
```
* `npm run build`, `npm run preview` 설명
* `npm ls`, `npm outdated(설치된 패키지 보다 새로운 버전의 패키지들이 있는지 확인)` 설명

<!--
### Static site generation
* https://svelte.dev/docs/kit/adapter-static
```sh
npm i -D @sveltejs/adapter-static
```
svelte.config.js
```js
import adapter from '@sveltejs/adapter-static';

export default {
	kit: {
		adapter: adapter({
			pages: 'build',
			assets: 'build',
			fallback: undefined,
			precompress: false,
			strict: true,
			fallback: 'index.html'
		})
	}
};
```
-->

## GIT
소스 관리를 위해 사용한다. 어느 파일이 언제 어떻게 변경되었는지 쉽게 볼 수 있다.

**GIT 설치**

**VSCode 확장 Git Graph 설치**

```sh
git init
```

## Markup
src/routes/+layout.svelte
```svelte
<link rel="stylesheet" href="/css/global.css" />
```
```diff
- {@render children?.()}
```
```svelte
<div id="app">
  <header>
    <h1>Svelte study</h1>
  </header>
  <hr />
  <div class="container">
    <nav class="nav">
      <ul>
        <li><h2>Users</h2></li>
        <li><h2>Search</h2></li>
      </ul>
    </nav>
    <hr />
    <section class="contents">
      <div>
        <h3>Users</h3>
        <p>Contents</p>
      </div>
    </section>
    <hr />
  </div>
  <footer>Copyright</footer>
</div>
```

static/css/global.css
```css
* {
  margin: 0;
  font-family: -apple-system,BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
}
a:link, a:visited {
  text-decoration: none;
  color: black;
}
a.active {
  color: white;
}
h1, footer, .nav ul {
  padding: 0.5rem;
}
h4, li {
  margin: 0.5rem 0;
}
hr {
  display: none;
  margin: 1rem 0;
  border: 0;
  border-top: 1px solid #ccc;
}
input[type=text] {
  width: 120px;
}

.d-block {
  display: block;
}
.container {
  display: flex;
  border-top: 1px solid #ccc;
  border-bottom: 1px solid #ccc;
}
.nav {
  min-height: 300px;
  background-color: #4285F4;
}
.nav ul {
  list-style: none;
}
.contents {
  flex: 1;
  padding: 1rem;
}

.table-search {
  border: 1px solid rgb(118, 118, 118);
  border-collapse: collapse;
  text-align: center;
}
.table-search th, .table-search td {
  padding: 0.2rem;
}
.table-search td {
  border-top: 1px solid rgb(118, 118, 118);
  min-width: 100px;
}
```

## Svelte Component 만들기
Header.svelte, Nav.svelte, Footer.svelte 이렇게 Component 별로 파일을 나눈다.

src/App.svelte
```svelte
<script>
import Header from './components/Header.svelte';
import Nav from './components/Nav.svelte';
import Footer from './components/Footer.svelte';
</script>
```

src/components/Header.svelte
```html
<header>
  <h1>Svelte study</h1>
</header>
```

src/App.svelte
```diff
- <header>
-  <h1>Svelte study</h1>
- </header>
+ <Header></Header>

- <nav class="nav">
-   <ul>
-     <li><h2>Users</h2></li>
-     <li><h2>Search</h2></li>
-   </ul>
- </nav>
+ <Nav></Nav>

- <footer>Copyright</footer>
+ <Footer></Footer>
```

### #if, props
src/App.svelte
```svelte
{#if false}
<Header></Header>
{/if}

<Footer title={'카피라이트'}></Footer>
```

src/components/Footer.svelte (덮어 씌우기)
```svelte
<script>
export let title;
</script>

<footer>{title || 'Copyright'}</footer>
```

## svelte-spa-router
https://github.com/ItalyPaleAle/svelte-spa-router
<!--
https://www.routify.dev
-->

### 설치
```sh
npm install svelte-spa-router
```

### Router 만들기
src/routes.js
```js
import Home from './components/contents/Home.svelte';
import Users from './components/contents/Users.svelte';
import Search from './components/contents/Search.svelte';

const routes = {
  '/': Home,
  '/users': Users,
  '/search': Search
};

export default routes;
```

src/components/contents/Home.svelte
```html
<script>
import {replace} from 'svelte-spa-router';

replace('/users');
</script>
```
* `replace`는 주소 창에서 `/#/home` history가 남지 않는다.

src/components/contents/Users.svelte
```html
<div>
  <h3>Users</h3>
  <p>Contents</p>
</div>
```

src/components/contents/Search.svelte (동일)

src/App.svelte
```js
import Router from 'svelte-spa-router';
import routes from './routes';
```
```diff
  <section class="contents">
-   <div>
-     <h3>Users</h3>
-     <p>Contents</p>
-   </div>
+   <Router {routes} />
```

**주소 창에서 router 바꾸어 보기**
* http://localhost:8080/#/users

src/components/Nav.svelte (덮어 씌우기)
```svelte
<script>
import {link} from 'svelte-spa-router'
import active from 'svelte-spa-router/active'
</script>

<nav class="nav">
  <ul>
    <li><h2><a href="/users" use:link use:active>Users</a></h2></li>
    <li><h2><a href="/search" use:link use:active>Search</a></h2></li>
    <!-- <li href="/search" use:active><h2><a href="/search" use:link use:active>Search</a></h2></li> li에 active를 넣어야 하는 경우 -->
  </ul>
</nav>
```

**여기 까지가 Markup 개발자 분들이 할일 입니다.**

## Users Store 만들기
**Store 개념 설명**

Component가 사용하는 글로벌 함수 또는 변수라고 생각하면 쉽다, state 값이 변하면 해당 값을 바라 보는 모든 Component가 수정 된다.

**Store 사용 하는 이유**

Component에 변경된 사항을 다시 그리기 위해서 Store를 사용 한다.

src/stores/usersStore.js
```js
import { writable } from 'svelte/store';

class UsersStore {
  users = writable([]);
  user = writable({
    name: '',
    age: ''
  });
}

export default new UsersStore();
```

### Users Component Store 주입
src/components/contents/Users.svelte
```svelte
<script>
import usersStore from '../../stores/usersStore.js';

const {users, user} = usersStore;
console.log($users, $user);
</script>

<div>
  <h3>Users</h3>
  <hr class="d-block" />
  <div>
    <h4>Read</h4>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Age</th>
          <th>Modify</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>홍길동</td>
          <td>20</td>
          <td>
            <button>Update</button>
            <button>Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <hr class="d-block" />
  <div>
    <h4>Create</h4>
    <input type="text" placeholder="Name" />
    <input type="text" placeholder="Age" />
    <button>Create</button>
  </div>
</div>
```

## Users Store CRUD
### Cread
src/stores/usersStore.js
```js
usersCreate(user) {
  this.users.update(users => {
    users.push({
      name: user.name,
      age: user.age
    });
    console.log('Done usersCreate', users);
    return users;
  });
};
```

src/components/contents/Users.svelte
```svelte
<input type="text" placeholder="Name" bind:value={$user.name} />
<input type="text" placeholder="Age" bind:value={$user.age} />
<button on:click="{() => usersStore.usersCreate($user)}">Create</button>
```

* `Footer.svelte` Store 적용 해보기
```svelte
<footer>Copyright {$user.name}</footer>
```

### Read
src/stores/usersStore.js
```js
usersRead() {
  this.users.update(() => {
    const users = [{
      name: '홍길동',
      age: 20
    }, {
      name: '춘향이',
      age: 16
    }];
    console.log('Done usersRead', users);
    return users;
  });
};
```

src/components/contents/Users.svelte
```js
usersStore.usersRead();
```
```diff
- <tr>
-   <td>홍길동</td>
-   <td>20</td>
-   <td>
-     <button>Update</button>
-     <button>Delete</button>
-   </td>
- </tr>
```
```svelte
{#each $users as user, index}
  <tr>
    <td>{user.name}</td>
    <td>{user.age}</td>
    <td>
      <button>Update</button>
      <button>Delete</button>
    </td>
  </tr>
{/each}
```

### Delete
src/stores/usersStore.js
```js
usersDelete(index) {
  this.users.update(users => {
    users.splice(index, 1);
    console.log('Done usersDelete', users);
    return users;
  });
};
```

src/components/contents/Users.svelte
```diff
- <button>Delete</button>
+ <button on:click="{() => usersStore.usersDelete(index)}">Delete</button>
```

### Update
src/stores/usersStore.js
```js
usersUpdate(index, user) {
  this.users.update(users => {
    users[index] = user;
    console.log('Done usersUpdate', users);
    return users;
  });
};
```

src/components/contents/Users.svelte
```diff
- <td>{{user.name}}</td>
- <td>{{user.age}}</td>
```
```svelte
<td><input type="text" placeholder="Name" bind:value={user.name} /></td>
<td><input type="text" placeholder="Age" bind:value={user.age} /></td>
```
```diff
- <button>Update</button>
```
```svelte
<button on:click="{() => usersStore.usersUpdate(index, user)}">Update</button>
```

## Backend Server
* [Download](https://github.com/ovdncids/vue-curriculum/raw/master/download/express-server.zip)
```sh
# BE 서버 실행 방법
npm install
node index.js
# 터미널 종료
Ctrl + c
```

## Axios 서버 연동
https://github.com/axios/axios
```sh
npm install axios
```

### Axios common 에러 처리
src/stores/common.js
```js
export const axiosError = (error) => {
  console.error(error.response || error.message || error);
};
```

### Create
src/stores/UsersStore.js
```js
import axios from 'axios';
import { axiosError } from './common.js';
```
### Create
src/stores/UsersStore.js
```js
import axios from 'axios';
import { axiosError } from './common.js';
```
```diff
usersCreate(user) {
- this.users.update(users => {
-   users.push(user);
-   console.log('Done usersCreate', users);
-   return users;
- });
};
```
```js
axios.post('http://localhost:3100/api/v1/users', user).then((response) => {
  console.log('Done usersCreate', response);
  this.usersRead();
}).catch((error) => {
  axiosError(error);
});
```

### Read
src/stores/UsersStore.js
```diff
usersRead() {
- this.users.update(() => {
-   const users = [{
-     name: '홍길동',
-     age: 20
-   }, {
-     name: '춘향이',
-     age: 16
-   }];
-   console.log('Done usersRead', users);
-   return users;
- });
};
```
```js
axios.get('http://localhost:3100/api/v1/users').then((response) => {
  console.log('Done usersRead', response);
  this.users.set(response.data.users);
}).catch((error) => {
  axiosError(error);
});
```

### Delete
src/stores/UsersStore.js
```diff
usersDelete(index) {
- this.users.update(users => {
-   users.splice(index, 1);
-   console.log('Done usersDelete', users);
-   return users;
- });
};
```
```js
axios.delete('http://localhost:3100/api/v1/users/' + index).then((response) => {
  console.log('Done usersDelete', response);
  this.usersRead();
}).catch((error) => {
  axiosError(error);
});
```

### Update
src/stores/UsersStore.js
```diff
usersUpdate(index, user) {
- this.users.update(users => {
-   users[index] = user;
-   console.log('Done usersUpdate', users);
-   return users;
- });
};
```
```js
axios.patch('http://localhost:3100/api/v1/users/' + index, user).then((response) => {
  console.log('Done usersUpdate', response);
  this.usersRead();
}).catch((error) => {
  axiosError(error);
});
```

## Search Store 만들기
src/stores/searchStore.js
```js
import usersStore from './usersStore.js';
import axios from 'axios';
import { axiosError } from './common.js';

class SearchStore {
  searchRead(q) {
    const url = 'http://localhost:3100/api/v1/search?q=' + q;
    axios.get(url).then((response) => {
      console.log('Done searchRead', response);
      usersStore.users.set(response.data.users);
    }).catch((error) => {
      axiosError(error);
    });
  }
}

export default new SearchStore();
```

### Search Component Store 주입
src/components/contents/Search.svelte
```svelte
<script>
import usersStore from '../../stores/usersStore.js';
import searchStore from '../../stores/searchStore.js';

const {users} = usersStore;
searchStore.searchRead('');
console.log($users);
</script>

<div>
  <h3>Search</h3>
  <hr class="d-block" />
  <div>
    <form>
      <input type="text" placeholder="Search" />
      <button>Search</button>
    </form>
  </div>
  <hr class="d-block" />
  <div>
    <table class="table-search">
      <thead>
        <tr>
          <th>Name</th>
          <th>Age</th>
        </tr>
      </thead>
      <tbody>
      {#each $users as user, index}
        <tr>
          <td>{user.name}</td>
          <td>{user.age}</td>
        </tr>
      {/each}
      </tbody>
    </table>
  </div>
</div>
```

## Search Component에서만 사용 가능한 state값 적용
src/components/contents/Search.svelte
```js
let q = '';
const searchRead = (event) => {
  event.preventDefault();
  searchStore.searchRead(q);
};
```
```diff
- <form>
-   <input type="text" placeholder="Search" />
-   <button>Search</button>
- </form>
```
```svelte
<form on:submit="{(event) => searchRead(event)}">
  <input type="text" placeholder="Search" bind:value={q} />
  <button>Search</button>
</form>
```

## Search Component 쿼리스트링 변경
src/components/contents/Search.svelte
```diff
+ import {push} from 'svelte-spa-router';
```

```diff
- searchStore.searchRead(q);
+ push('/search?q=' + q);
```

src/components/Nav.svelte
```diff
- <li><h2><a href="/search" use:link use:active>Search</a></h2></li>
+ <li><h2><a href="/search" use:link use:active={/search*/}>Search</a></h2></li>
```
* `active` 조건은 `정규식`이 가능하다.
* `검색`, `뒤로가기` 해보기

## Search Component 새로고침 적용
```diff
- import {push} from 'svelte-spa-router';
+ import {push, querystring} from 'svelte-spa-router';
```
```diff
- searchStore.searchRead('');
```

```js
const watchQuerystring = (querystring) => {
  q = new URLSearchParams(querystring).get('q') || '';
  searchStore.searchRead(q);
}
$: watchQuerystring($querystring);
```

## Proxy 설정
* https://github.com/sveltejs/svelte/issues/3717

```sh
npm install rollup-plugin-dev --save-dev
```

rollup.config.js
```js
import dev from 'rollup-plugin-dev';

  plugins: [
    !production && dev({
      dirs: ['public'],
      host: 'localhost',
      port: 8080,
      proxy: [{
        from: '/api',
        to: 'http://localhost:3100/api'
      }]
    })
  ]
```

모든 파일 수정
```diff
- http://localhost:3100/api
+ /api
```

## SCSS 설정
* https://github.com/sveltejs/svelte-preprocess/blob/main/docs/getting-started.md
* https://jforj.tistory.com/179
* https://jforj.tistory.com/184

```sh
npm install -D svelte-preprocess node-sass postcss
```

src/App.scss
```scss
div {
  color: red;
}
```

rollup.config.js
```js
import sveltePreprocess from 'svelte-preprocess';

  plugins: [
    svelte({
      preprocess: sveltePreprocess()
    })
```

src/App.svelte
```svelte
<style global lang="scss">
@import 'src/App.scss';
</style>
```
* ❕ `모든 .svelte 파일`에서 사용할 `.scss 파일`은 `global`속성 안에서 선언 해야 한다.

#### `lang="scss"` 오류 설정
```sh
확장 > Svelte for VS Code > 확장 설정 > Svelte › Language-server: Ls-path
  Windows: C:\Program Files\nodejs
  Mac: /Users/사용자/.nvm/versions/node/v14.15.1/bin/node
```
* ❕ `Mac`은 터미널에서 `which node` 명령으로 `Node` 경로 확인 가능
* ❕ `VSCode` 재시작

# 수고 하셨습니다.
