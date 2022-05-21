# Svelte
* [데모](https://ovdncids.github.io/svelte-curriculum/demo)

## Node.js
https://nodejs.org

## NVM (Node Version Manager)
Node.js 버전을 관리하는 프로그램, 어느 버전이든 설치, 변경, 삭제 가능하다.

<details><summary>Mac OS</summary>
https://github.com/nvm-sh/nvm

https://gist.github.com/falsy/8aa42ae311a9adb50e2ca7d8702c9af1
```sh
# 설치
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash

# vi 에디터 실행
vi ~/.bash_profile

# 해당 경로 적용 시키키
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# ~/.bash_profile 재 실행 시키기
source ~/.bash_profile
```
</details>

Windows

https://github.com/coreybutler/nvm-windows/releases

```sh
# 설치 된 node.js 리스트를 본다.
nvm ls

# 해당 버전을 설치 한다.
nvm install 14.15.5

# 해당 버전을 삭제 한다.
nvm uninstall 14.15.5

# 해당 버전을 사용 한다.
nvm use 14.15.5

# 기본 버전 변경 한다.
nvm alias default 14.15.5
```

## Svelte CLI
* https://svelte.dev/docs
* https://v2.svelte.dev/guide
* https://beomy.github.io/tech/svelte/start-svelte
```sh
# Svelte 프로젝트 생성
npx degit sveltejs/template svelte-study
cd svelte-study
code .

# Typescript로 변경
node scripts/setupTypeScript.js
## src/main.ts
## import App from './App.svelte'; 오류 난다면 일단 무시 하자

# VSCode로 해당 디렉토리 열기
npm install
npm run dev
```
* `npm run build` 설명

## GIT
소스 관리를 위해 사용한다. 어느 파일이 언제 어떻게 변경 되었는지 쉽게 볼 수 있다.

**GIT 설치**

**VSCode 확장 Git Graph 설치**

```sh
git init
```

## Markup
src/App.svelte (덮어 씌우기)
```svelte
<script>
</script>

<div id="app">
  <header>
    <h1>Svelte study</h1>
  </header>
  <hr />
  <div class="container">
    <nav class="nav">
      <ul>
        <li><h2>Members</h2></li>
        <li><h2>Search</h2></li>
      </ul>
    </nav>
    <hr />
    <section class="contents">
      <div>
        <h3>Members</h3>
        <p>Contents</p>
      </div>
    </section>
    <hr />
  </div>
  <footer>Copyright</footer>
</div>
```

public/global.css (덮어 씌우기)
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
-     <li><h2>Members</h2></li>
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
import Members from './components/contents/Members.svelte';
import Search from './components/contents/Search.svelte';

const routes = {
  '/': Home,
  '/members': Members,
  '/search': Search
};

export default routes;
```

src/components/contents/Home.svelte
```html
<div>
  <h3>Home</h3>
  <p>Contents</p>
</div>
```
src/components/contents/Members.svelte (동일)

src/components/contents/Search.svelte (동일)

src/App.svelte
```js
import Router from 'svelte-spa-router';
import routes from './routes';
```
```diff
  <section class="contents">
-   <div>
-     <h3>Members</h3>
-     <p>Contents</p>
-   </div>
+   <Router {routes} />
```

**주소 창에서 router 바꾸어 보기**
* http://localhost:8080/#/members

src/components/Nav.svelte (덮어 씌우기)
```svelte
<script>
import {link} from 'svelte-spa-router'
import active from 'svelte-spa-router/active'
</script>

<nav class="nav">
  <ul>
    <li><h2><a href="/" use:link use:active>Home</a></h2></li>
    <li><h2><a href="/members" use:link use:active>Members</a></h2></li>
    <li><h2><a href="/search" use:link use:active>Search</a></h2></li>
  </ul>
</nav>
```

**여기 까지가 Markup 개발자 분들이 할일 입니다.**

## Members Store 만들기
**Store 개념 설명**

Component가 사용하는 글로벌 함수 또는 변수라고 생각하면 쉽다, state 값이 변하면 해당 값을 바라 보는 모든 Component가 수정 된다.

src/stores/membersStore.js
```js
import { writable } from 'svelte/store';

class MembersStore {
  members = writable([]);
  member = writable({
    name: '',
    age: ''
  });
}

export default new MembersStore();
```

### Members Component Store inject
src/components/contents/Members.svelte
```svelte
<script>
import membersStore from '../../stores/membersStore.js';

const {members, member} = membersStore;
console.log($members, $member);
</script>

<div>
  <h3>Members</h3>
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

## Members Store CRUD
### Cread
src/stores/membersStore.js
```js
membersCreate(member) {
  this.members.update(members => {
    members.push({
      name: member.name,
      age: member.age
    });
    console.log('Done membersCreate', members);
    return members;
  });
};
```

src/components/contents/Members.svelte
```svelte
<input type="text" placeholder="Name" bind:value={$member.name} />
<input type="text" placeholder="Age" bind:value={$member.age} />
<button on:click="{() => membersStore.membersCreate($member)}">Create</button>
```

* `Footer.svelte` Store 적용 해보기
```svelte
<footer>Copyright {$member.name}</footer>
```

### Read
src/stores/membersStore.js
```js
membersRead() {
  this.members.update(() => {
    const members = [{
      name: '홍길동',
      age: 20
    }, {
      name: '춘향이',
      age: 16
    }];
    console.log('Done membersRead', members);
    return members;
  });
};
```

src/components/contents/Members.svelte
```js
membersStore.membersRead();
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
{#each $members as member, index}
  <tr>
    <td>{member.name}</td>
    <td>{member.age}</td>
    <td>
      <button>Update</button>
      <button>Delete</button>
    </td>
  </tr>
{/each}
```

### Delete
src/stores/membersStore.js
```js
membersDelete(index) {
  this.members.update(members => {
    members.splice(index, 1);
    console.log('Done membersDelete', members);
    return members;
  });
};
```

src/components/contents/Members.svelte
```diff
- <button>Delete</button>
+ <button on:click="{() => membersStore.membersDelete(index)}">Delete</button>
```

### Update
src/stores/membersStore.js
```js
membersUpdate(index, member) {
  this.members.update(members => {
    members[index] = member;
    console.log('Done membersUpdate', members);
    return members;
  });
};
```

src/components/contents/Members.svelte
```diff
- <td>{{member.name}}</td>
- <td>{{member.age}}</td>
```
```svelte
<td><input type="text" placeholder="Name" bind:value={member.name} /></td>
<td><input type="text" placeholder="Age" bind:value={member.age} /></td>
```
```diff
- <button>Update</button>
```
```svelte
<button on:click="{() => membersStore.membersUpdate(index, member)}">Update</button>
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
src/stores/MembersStore.js
```js
import axios from 'axios';
import { axiosError } from './common.js';
```
### Create
src/stores/MembersStore.js
```js
import axios from 'axios';
import { axiosError } from './common.js';
```
```diff
membersCreate(member) {
- this.members.update(members => {
-   members.push(member);
-   console.log('Done membersCreate', members);
-   return members;
- });
};
```
```js
axios.post('http://localhost:3100/api/v1/members', member).then((response) => {
  console.log('Done membersCreate', response);
  this.membersRead();
}).catch((error) => {
  axiosError(error);
});
```

### Read
src/stores/MembersStore.js
```diff
membersRead() {
- this.members.update(() => {
-   const members = [{
-     name: '홍길동',
-     age: 20
-   }, {
-     name: '춘향이',
-     age: 16
-   }];
-   console.log('Done membersRead', members);
-   return members;
- });
};
```
```js
axios.get('http://localhost:3100/api/v1/members').then((response) => {
  console.log('Done membersRead', response);
  this.members.set(response.data.members);
}).catch((error) => {
  axiosError(error);
});
```

### Delete
src/stores/MembersStore.js
```diff
membersDelete(index) {
- this.members.update(members => {
-   members.splice(index, 1);
-   console.log('Done membersDelete', members);
-   return members;
- });
};
```
```js
axios.delete('http://localhost:3100/api/v1/members/' + index).then((response) => {
  console.log('Done membersDelete', response);
  this.membersRead();
}).catch((error) => {
  axiosError(error);
});
```

### Update
src/stores/MembersStore.js
```diff
membersUpdate(index, member) {
- this.members.update(members => {
-   members[index] = member;
-   console.log('Done membersUpdate', members);
-   return members;
- });
};
```
```js
axios.patch('http://localhost:3100/api/v1/members/' + index, member).then((response) => {
  console.log('Done membersUpdate', response);
  this.membersRead();
}).catch((error) => {
  axiosError(error);
});
```

## Search Store 만들기
src/stores/searchStore.js
```js
import axios from 'axios';
import { axiosError } from './common.js';
import membersStore from './membersStore.js';

class SearchStore {
  searchRead(q) {
    const url = `http://localhost:3100/api/v1/search?q=${q}`;
    axios.get(url).then((response) => {
      console.log('Done searchRead', response);
      membersStore.members.set(response.data.members);
    }).catch((error) => {
      axiosError(error);
    });
  }
}

export default new SearchStore();
```

src/components/contents/Search.svelte
```svelte
<script>
import membersStore from '../../stores/membersStore.js';
import searchStore from '../../stores/searchStore.js';

const {members} = membersStore;
searchStore.searchRead('');
console.log($members);
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
      {#each $members as member, index}
        <tr>
          <td>{member.name}</td>
          <td>{member.age}</td>
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

## Search Compenent 쿼리스트링 변경과 새로고침 적용
src/components/contents/Search.svelte
```diff
+ import {push, querystring} from 'svelte-spa-router';
```
```diff
- searchStore.searchRead('');
```
```diff
- searchStore.searchRead(q);
+ push(`/search?q=${q}`);
```
* `검색`, `뒤로가기` 해보기

```js
const watchQuerystring = (querystring) => {
  q = new URLSearchParams(querystring).get('q') || '';
  searchStore.searchRead(q);
}
$: watchQuerystring($querystring);
```

### Navigator active 수정
src/components/Nav.svelte
```diff
- <li><h2><a href="/search" use:link use:active>Search</a></h2></li>
+ <li><h2><a href="/search" use:link use:active={/search*/}>Search</a></h2></li>
```
* `active` 조건은 `정규식`이 가능하다.

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
