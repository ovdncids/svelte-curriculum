# Svelte

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
  <h1>Vue.js study</h1>
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
