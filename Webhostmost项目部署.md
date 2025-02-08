Webhostmost项目部署



1. [注册Webhostmost](https://client.webhostmost.com/register.php)，邮箱认证后，点击**Buy New Hosting Plan**，选择左上角**Free Plan**，点击**Order Now**.

![image-20250208163623024](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208163623024.png)

![image-20250208163713440](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208163713440.png)

2.跳转到域名选项，选择第三个**Use Subdomain**（选择官方提供的免费域名.freewebhostmost.com），输入自己的域名前缀，例如：**hezi**.freewebhostmost.com ，点击Check创建自己的项目访问链接。

![image-20250208164443027](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208164443027.png)

3.官方提供**永久免费**、**125M**的磁盘空间和**5个国家**地区（官方地区数据混乱，哪吒显示只有三个地区），服务器位置随自己的爱好选择，跳转到支付页面，点击右边**Checkout**.（125M空间部署此项目足够）

![image-20250208165024773](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208165024773.png)

![image-20250208165210326](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208165210326.png)

![image-20250208165454546](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208165454546.png)

4.回到首页即可看到服务器信息，点击**Go To Control Panel**跳转到后台管理页面，点击左栏**Files Management** ➡**File Management**➡domains➡xxx.freewebhostmost.com➡public_html.   将项目中的**app.js**和**package.json**上传到此目录下即可。

![image-20250208170323752](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208170323752.png)

![image-20250208170455516](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208170455516.png)

5.打开**app.js**修给几处参数数据即可，**UUID** **NEZHA_SERVER** **NEZHA_PORT** **NEZHA_KEY** **DOMAIN**  **PORT**

哪吒数据三件套就不多说了，有哪吒的都会；如果你没有哪吒可不填。

UUID 如有必要也可以替换新的；**DOMAIN**  填写分配的域名可保活，以防万一也可以放在哪吒服务中。

**PORT**  **重点** **重点** **重点** 说三遍，不建议默认3000端口，端口一定会占用，端口可随便填写。

![。](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208170639146.png)

6.点击左栏**Website Management**➡**NodeJs APP**➡**Create application**➡**CREATE**

Node.js version➡**v22**

Application root➡**domains/xxx.freewebhostmost.com/public_html**  (替换自己的完整域名)

Application startup file➡**app.js**

![image-20250208171625900](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208171625900.png)

![image-20250208172048012](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208172048012.png)

7.回到**Development Tools**➡**terminal**。刚才创建完成**Node.js**应用后会页面出现一行命令，将此命令复制到**terminal**中回车获取权限，之后再输入 **npm i**安装依赖。

![image-20250208172608534](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208172608534.png)

![image-20250208173317923](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208173317923.png)

8.回到**Node.js应用**界面，点击**Run JS script**

![image-20250208173408571](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208173408571.png)

![image-20250208173439307](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208173439307.png)

9.输入你的域名/sub即可获取节点。例如https://hezi.freewebhostmost.com/sub，如果你之前没有反代此域名请你用原域名，节点如下：

`vless://b28f60af-d0b9-4ddf-baaa-7e49c93c380b@hezi.freewebhostmost.com:443?encryption=none&security=tls&sni=hezi.freewebhostmost.com&allowInsecure=1&type=ws&path=%2F#USA-webhostmost-GCP`



如果想workers反代请使用一下代码：二选一

```js
export default {
    async fetch(request, env) {
        let url = new URL(request.url);
        if (url.pathname.startsWith('/')) {
            var arrStr = [
                'aaaa.bbbbb.hf.space',// 修改成自己的节点IP/域名
            ];
            url.protocol = 'https:'
            url.hostname = getRandomArray(arrStr)
            let new_request = new Request(url, request);
            return fetch(new_request);
        }
        return env.ASSETS.fetch(request);
    },
};
function getRandomArray(array) {
  const randomIndex = Math.floor(Math.random() * array.length);
  return array[randomIndex];
}
```



```js
export default {
  async fetch(request, env, ctx) {
    let url = new URL(request.url);
    if(url.pathname.startsWith('/')){
      url.hostname="translate.google.com"; // 修改成自己的节点IP/域名
      let new_request = new Request(url, request)
      return await fetch(new_request)
    }
    return await env.ASSETS.fetch(request);
  },
};
```









10.来到哪吒即可看到哪吒已经点亮。

![image-20250208173721814](C:\Users\科技小岛\AppData\Roaming\Typora\typora-user-images\image-20250208173721814.png)