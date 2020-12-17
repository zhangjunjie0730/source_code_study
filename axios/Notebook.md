# API使用

## ⭐axios.creator()

1. 根据指定配置创建一个新的 axios, 也就就每个新 axios 都有自己的配置

2. 新 axios 只是没有取消请求和批量发请求的方法, 其它所有语法都是一致的

3. 为什么要设计这个语法?

   - 需求: 项目中有部分接口需要的配置与另一部分接口需要的配置不太一样, 如何处理
   - 解决: 创建 2 个新 axios, 每个都有自己特有的配置, 分别应用到不同要求的接口请求中  

```js
const instance = axios.create({ baseURL: 'http://localhost:3000' });
// 使用instance发请求
instance.get({ url: '/posts' });
```

比如说，一个axios调用 localhost:3000 接口，另一个调用 localhost:4000 接口，创建了一个衍生，方便配置 baseURL

### 源码



## ⭐axios.interceptors()

**流程: 请求拦截器2 => 请求拦截器 1 => 发ajax请求 => 响应拦截器1 => 响应拦截器 2 => 请求的回调  **

注意：请求拦截器是后添加的先执行！！！

```js
// 添加请求拦截器(回调函数)
    axios.interceptors.request.use(
      config => {
        console.log('request interceptor1 onResolved()')
        return config //一定要返回config，实际上promise的串联工作，如果不反悔，就给下一个promise对象传入了undefined
      },
      error => {
        console.log('request interceptor1 onRejected()')
        return Promise.reject(error);
      }
    )
    axios.interceptors.request.use(
      config => {
        console.log('request interceptor2 onResolved()')
        return config
      },
      error => {
        console.log('request interceptor2 onRejected()')
        return Promise.reject(error);
      }
    )
    // 添加响应拦截器
    axios.interceptors.response.use(
      response => {
        console.log('response interceptor1 onResolved()')
        return response
      },
      function (error) {
        console.log('response interceptor1 onRejected()')
        return Promise.reject(error);
      }
    )
    axios.interceptors.response.use(
      response => {
        console.log('response interceptor2 onResolved()')
        return response
      },
      function (error) {
        console.log('response interceptor2 onRejected()')
        return Promise.reject(error);
      }
    )

    axios.get('http://localhost:3000/posts')
      .then(response => {
        console.log('data', response.data)
      })
      .catch(error => {
        console.log('error', error.message)
      })
// 输出：
'request interceptor2 onResolved()'
'request interceptor1 onResolved()'
'response interceptor1 onResolved()'
'response interceptor2 onResolved()'
'data', response.data 最后执行请求的回调！
```



### 源码



## 取消请求





















