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

3. 其他 nginx反向代理, node中间件, iframe + location.hash ...等等


