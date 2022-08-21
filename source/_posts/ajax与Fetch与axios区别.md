### 三者都用于网络请求、但是不同维度

Ajax ，一种技术统称

Fetch，一个具体的API

Axios，一个npm库（内部可用XMLHttpRequest、Fetch实现）





#### XMLHttpRequest封装Ajax

```javascript
function ajax (url, method, body, headers) {
  return new Promise((resolve, reject) => {
    let request = new XMLHttpRequest();
    request.open(method, url); // 初始化一个请求
      
    for (let key in headers) {
      request.setRequestHeader(key, headers[key]); // 设置HTTP请求头部的方法，该方法必须在 open()之后，send() 之前调用
    }
      
    request.onreadystatechange = () => {
      if (request.readyState === 4) {
        if (request.status === 200 || request.status === 304) {
          resolve(request.responseText); // request.responseText 是一个字符串，需要 JSON.parse() 转换
        } else {
          reject(request);
        }
      }
    }

    request.send(body); // 发送http请求
  })
}


ajax('./data.json', 'get').then(res => {
  console.log(res)
})
```



#### Fetch封装Ajax

```javascript
function ajax(url, method, body, headers) {
    fetch(url, {
        method,
        headers,
        body
    }).then((res) => {
        if (res.status === 200) {
            return res.json()
        } else {
            return Promise.reject(res.json())
        }
    }).then(function(data) {
        console.log(data);
    }).catch(function(err) {
        console.log(err);
    });
}

ajax('./data.json', 'get').then(res => {
  console.log(res)
})
```

