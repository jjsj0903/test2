# test2# 뷰 라우터

##라우팅

> 웹 페이지 이동 방법

라우팅을 사용하는 이유

```markdown
사용자가 웹 페이지 요청 > 서버에서 응답을 받아 웹 페이지 로드
**해당 시간 동안 화면 상에 깜빡거림 현상 발생**
뷰, 리액트, 앵귤러 모두 라우팅을 이용해 화면을 전환
```



## 뷰 라우터

>  뷰 라이브러리를 이용하여 **싱글 페이지 애플리케이션**을 구현할 때 사용하는 라이브러리

```markdown
싱글 페이지 애플리케이션란?
페이지를 이동할 때마다 서버에 웹 페이지를 요청해 갱신하는 것이 아니라 
미리 해당 페이지를 받아 놓고 페이지 이동 시에 클라언트의 화면을 갱신하는 패턴을 적용한 애플리케이션 
```



##뷰 라우터 설치

**CDN 방식**

```
<script src="https://unpkg.com/vue-router/dist/vue-router.js">
```

**npm 방식**

`````
npm install vue-router
`````



##뷰 라우터 태그 & 기능

* `<router-link to="url 값">`: 페이지 이동 태그

  `<router-link>`는 `<a>` 태그로 렌더링

* `<router-view>`: 페이지 표시 태그, 변경되는 url에 따라 해당 컴포넌트를 뿌려주는 영역



> HTML

```html
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <router-link to="/main">main 버튼</router-link>
    <router-link to="/login">login 버튼</router-link>
  </p>
  <router-view></router-view>
</div>
```

> JS

```html
 <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
 <script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>
  <script>
      // Main, Login 컴포넌트 내용 정의
      var Main = { template: '<div>main</div>' };
      var Login = { template: '<div>login</div>' };
      // 각 url에 해당하는 컴포넌트 등록
      var routes = [
        { path: '/main', component: Main },
        { path: '/login', component: Login }
      ];
      // 뷰 라우터 정의
      var router = new VueRouter({
        routes
      });
      // 뷰 라우터를 인스턴스에 등록
      var app = new Vue({
        router
      }).$mount('#app');
  </script>
  </body>
```

[go](https://codepen.io/sujeong0903/pen/YzKZGyy )

> mount란?
>
> el 속성과 동일하게 **인스턴스를 화면에 붙이는 역할**

```
Vue 인스턴스가 인스턴스화 할 때 el 옵션이 없으면 연결된 DOM 엘리먼트 없이 “unmounted” 상태가 됩니다. vm.$mount()는 unmounted 된 Vue인스턴스의 마운트를 수동으로 시작하는데 사용할 수 있습니다.
```

[참고](https://kr.vuejs.org/v2/api/index.html#vm-mount)



## 뷰 라우터 옵션

* **mode** : URL의 해쉬 값 제거 속성

  - hash  (기본값)

    ```
     URL에서 해시(#) 기호 다음의 경로는 페이지 내부의 이름으로 표시. 
     따라서 해시 이후의 경로가 바뀌더라도 페이지가 다시 로드되지 않음.
    ```

  - history

    ```markdown
    해시를 제거하기 위한 방법
    history.pushState API를 사용하여 페이지를 다시 로드하지 않고 URL을 변경.
    ```

    ```js
    var router = new VueRouter({
       mode: 'history',
       routes
    });
    ```

    [issue](https://router.vuejs.org/kr/guide/essentials/history-mode.html#%EC%84%9C%EB%B2%84-%EC%84%A4%EC%A0%95-%EC%98%88%EC%A0%9C )

  

* **routes** : 라우팅 할 URL과 컴포넌트 값 지정 

  ```js
  var router = new VueRouter({
      mode: 'history',
      routes: [
        { path: '/login', component: Login },
        { path: '/main', component: Main }
      ]
  });
  var app = new Vue({
       router
  }).$mount('#app');
  ```

* **children**:  `routes`와 같은 라우트 설정 객체의 또 다른 배열. 중첩 된 뷰 URL과 컴포넌트 값 지정 

  

## 네스티드 라우터 (중첩된 라우터)

> 상위 컴포넌트 1개에 하위 컴포넌트 1개를 포함하는 구조 

```
/user/foo/profile                     /user/foo/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```



```html
<div id="app">
   <!--user가 뿌려질 영역-->
   <router-view></router-view>
</div>
```

```js
var User = {
    template: `
      <div class="user">
        <h1>User Component</h1>
        <!--하위 컴포넌트가 뿌려질 영역-->
        <router-view></router-view>
       </div>
     `
 };
var UserProfile = { template: '<p class="child">User Profile Component</p>' };
var UserPost = { template: '<p class="child">User Post Component</p>' };
var routes = [{
     path: '/user',
     component: User,
     children: [{
          path: 'posts',
          component: UserPost
        },
        {
           path: 'profile',
           component: UserProfile
        }]
 }];
```

컴포넌트 수가 적을 땐 유용, 한번에 많은 컴포넌트를 표시할 때에는 **네임드 뷰** 사용



## 네임드 뷰

> 같은 레벨에서 여러 개의 컴포넌트를 한 번에 표시하는 구조

```
+------------------+                              
| +--------------+ |               
| | header       | |  
| +--------------+ |
| +--------------+ |               
| | body         | |  
| +--------------+ |
| +--------------+ |               
| | footer       | |  
| +--------------+ |
+------------------+             
```



```html
<div id="app">
  <router-view name="header"></router-view>
  <router-view></router-view> <!--name 속성이 없으면 default-->
  <router-view name="footer"></router-view>
</div>
```

```js
var Body = { template: '<div>This is Body</div>' };
var Header = { template: '<div>This is Header</div>' };
var Footer = { template: '<div>This is Footer</div>' };
var router = new VueRouter({
     routes: [
     {
         path: '/',
         components: {
            default: Body,
            header: Header,
            footer: Footer
         }
     }]
})
```

[뷰 라우터](https://router.vuejs.org/kr/ )

--------

# 뷰HTTP 통신 

## 뷰 리소스

> Vue에서 HTTP 통신을 위해 제공하는 플러그인 

```js
new Vue({
  el: '#app',
  methods: {
    getData: function() {
      this.$http.get(`https://raw.githubusercontent.com/joshua1988/doit-vuejs/master/data/demo.json`)
          .then(function(response) {
            console.log(response);
      });
    }
  }
 });
```



## 액시오스

> 뷰에서 권고하는 HTTP 통신 라이브러리

[axios](https://github.com/axios/axios )

###설치

**CDN 방식**

```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

**npm 방식**

```
npm install axios
```



### 사용법

> `axios` 변수를 이용

* ` axios.get('URL주소').then().catch() `
  *  해당 URL로 get방식으로 요청.
  * then()안에 반환값 로직 작성. 
  * catch()안에는 오류발생시 로직 작성.  
*  `axios.post('URL주소').then().catch() `
  -  해당 URL로 post방식으로 요청.
  - then()안에 반환값 로직 작성. 
  - catch()안에는 오류발생시 로직 작성. 

```js
new Vue({
    el: '#app',
    methods: {
        getData: function() {
			axios.get('https://raw.githubusercontent.com/joshua1988/doit-vuejs/master/data/demo.json')
				.then(function(response) {
					console.log(response);
				});
            }
        }
});
```
