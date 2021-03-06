# vue-admin

`yarn add -D sass-loader node-sass`

## ui

main.js

```js
import Element from 'element-ui'
import './styles/element-variables.scss'
```

login 滿版

`yarn add normalize.css`

```js
import 'normalize.css/normalize.css' // a modern alternative to CSS resets
```

## mock

mockjs

vue.config.js

```js
  devServer: {
    port: port,
    open: true,
    overlay: {
      warnings: false,
      errors: true
    },
    before: require('./mock/mock-server.js')
  },
```

mock\mock-server.js

```js
// for mock server
const responseFake = (url, type, respond) => {
  return {
    url: new RegExp(`${process.env.VUE_APP_BASE_API}${url}`),
    type: type || 'get',
    response(req, res) {
      console.log('request invoke:' + req.path)
      res.json(Mock.mock(respond instanceof Function ? respond(req, res) : respond))
    }
  }
}
```

.env.development

```js
# base api
VUE_APP_BASE_API = '/dev-api'
```

mock\user.js

```js
  // get user info
  {
    url: '/vue-element-admin/user/info\.*',
    type: 'get',
```

http://localhost:8080/dev-api/vue-element-admin/user/info
> {"code":50008,"message":"Login failed, unable to get user details."}

http://localhost:8888/dev-api/vue-element-admin/user/login
> {"code":20000,"data":{"token":"admin-token"}}

## login

src\views\login\index.vue

```js
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.loginForm)  //api
```

src\store\modules\user.js

```js
import { login, logout, getInfo } from '@/api/user'
const actions = {
  // user login
  login({ commit }, userInfo) {
    const { username, password } = userInfo
    return new Promise((resolve, reject) => {
      login({ username: username.trim(), password: password }).then(response => {
        const { data } = response
        commit('SET_TOKEN', data.token)
        setToken(data.token)
        resolve()
      }).catch(error => {
        reject(error)
      })
    })
  },
```

src\api\user.js

```js
import request from '@/utils/request'
export function login(data) {
  // http://localhost:8888/dev-api/vue-element-admin/user/login

  return request({
    url: '/vue-element-admin/user/login',
    method: 'post',
    data
  })
}
```

src\utils\request.js

```js
// create an axios instance
const service = axios.create({
  baseURL: process.env.VUE_APP_BASE_API, // url = base url + request url
  // withCredentials: true, // send cookies when cross-domain requests
  timeout: 5000 // request timeout
})
```

## Sidebar

store\modules\permission.js

```js
import { asyncRoutes, constantRoutes } from '@/router'

 state.routes = constantRoutes.concat(routes)
```

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
