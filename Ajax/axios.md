# axios

> axios 是一个专注于网络请求的库！

## 简单请求

```js
const res = axios({
    method: 'GET',
    url: 'http://www.liuhongbin.top:3006/api/getbooks'
    data: {
    	name: 'zs',
    	age: '20'
	}
})
```

注意：

- axios 获取到的 res 并不是请求到的数据，请求到的数据在 data 属性中。（`res.data`)

```js
const res = axios({
    method: 'GET',
    url: 'https://mock.apifox.cn/m1/1301638-0-default/pet/1'
}).then( pet => {
    console.log(pet)
})
/*
api 返回的正常结果：

{"code":0,"data":{"id":"1","name":"层称府","photoUrls":["http://dummyimage.com/200x200"],"category":{"id":5632037461384914,"name":"Cat"},"tags":[{"id":1219386706259428,"name":"cat"}],"status":"available"}}


axios 返回的封装结果：

    config: {transitional: {…}, transformRequest: Array(1), transformResponse: Array(1), timeout: 0, adapter: ƒ, …}
    data:
        code: 0
        data: {id: '1', name: '它半何', photoUrls: Array(1), category: {…}, tags: Array(1), …}
        [[Prototype]]: Object
    headers: {content-length: '208', content-type: 'application/json; charset=utf-8', date: 'Fri, 19 Aug 2022 01:56:46 GMT', x-fc-code-checksum: '14901253298633711047', x-fc-invocation-duration: '71', …}
    request: XMLHttpRequest {onreadystatechange: null, readyState: 4, timeout: 0, withCredentials: false, upload: XMLHttpRequestUpload, …}
    status: 200
    statusText: "OK"
    [[Prototype]]: Object
*/
```

## async 和 await

如果调用某个方法返回值是 Promise 实例，则前面可以加 await。这样就不用 then 就可以直接打印了。

await 只能用在被 async 修饰的方法中。如果方法不声明 async 则会报错

```js
let apiGet = async function(){
  const res = await axios({
    method: 'GET',
    url: 'https://mock.apifox.cn/m1/1301638-0-default/pet/1'
  })
  console.log(res);
}
apiGet();
```

## 结构赋值获取目标数据

```js
async function apiGet() {
  const { data: res } = await axios({
    method: 'GET',
    url: 'https://mock.apifox.cn/m1/1301638-0-default/pet/1'
  })
  console.log(res);
}
apiGet();
```

