### 跨域解决方案
1. jsonp原理，利用`<script>`标签没有跨域限制的漏洞
    缺点：数据格式有要求，需要服务端配合，只有get请求，可能会遭到xss攻击
    示例：
    ```
    function jsonp({ url, params, callback }) {
        return new Promise((resolve, reject) => {
            let script = document.createElement('script')
            window[callback] = function(data) {
            resolve(data)
            document.body.removeChild(script)
            }
            params = { ...params, callback } // wd=b&callback=show
            let arrs = []
            for (let key in params) {
            arrs.push(`${key}=${params[key]}`)
            }
            script.src = `${url}?${arrs.join('&')}`
            document.body.appendChild(script)
        })
    }
    jsonp({
        url: 'http://localhost:3000/say',
        params: { wd: 'Iloveyou' },
        callback: 'show'
    }).then(data => {
        console.log(data)
    })

    ```

2. `iframe` 配合window.name 

    例如：  
    同源下的 `a.html` 和 `b.html`,不同源头下的 `c.html`, 现在在 `a.html` 要获取 `c.html` 的数据，此时跨域
    于是可以在 `a.html` 下创建一个 `iframe` 请求 `c.html` , `c.html` 将想要传递的数据包装在 `window.name` 中,然后src指向 `b.html` 此时 `b.html` 就能通过 `window.name` 获取到数据，然后父级 `a.html` 得到并使用

3.  webpack开发环境proxy代理
     webpack开发环境中，可以在packge.json里配置一个代理 `proxy`，值为你想要代理的存在跨域的接口，原理是只有浏览器有同源策略，看例子

    ```
    // 跨域
    //本地开发环境地址 http://127.0.0.1:8080 , 服务器地址：https://www.liwend.cn  存在跨域
    ajax.get('https://www.liwend.cn/test')...  // error ,跨域报错
    // webpack开发环境下解决办法
    // 在packge.json 文件里配置 proxy:https://www.liwend.cn
    // packge.json
    ...
    proxy:https://www.liwend.cn,
    // 客户端请求时不再请求 https://www.liwend.cn 这个地址，改为请求本地不跨域地址，即 http://127.0.0.1:8080/test
    ajax.get('/test') // 理论上会报错，找不到接口，但是由于配置了 proxy ,此时请求 http://127.0.0.1:8080/test 被代理请求到了 https://www.liwend.cn/test ，由于只有浏览器存在同源策略，所以就可以获取数据成功！
    ```

4. nginx反向代理 
    第3种方法只适用于开发环境，毕竟上线了线上没有webpack,所以，线上可以使用nginx反向代理，原理是同样是只有浏览器存在同源策略：
    客户端向代理服务器发起请求，代理服务器收到请求后用服务器环境请求数据服务器，由于只有浏览器存在同源策略，所以代理服务器可以拿到数据，代理服务器拿到数据后再把数据发送给客户端,这！就是反向代理

5. 其他 nginx反向代理, node中间件, iframe + location.hash ...等等