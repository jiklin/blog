title: 一个简单的http请求
date: 2015-09-11 12:06:24
categories: skills
tags: [NodeJs, 10danteng]
---

看[蛋图](http://10danteng.com/)好久了，偶尔也会发现逼高的点赞量，作为`写代码的`我能发现不了是作弊的嘛！2333
又正好最近在学 nodejs, so, 决定也写了个，练练手。这个还是相当简单滴 :)

## 分析
常规的操作是限制评论次数的，作为一个程序员怎么会按正确的姿势打开呢，查看chrome控制台
![](/upload/10dantu/01.png)
发现，在点击`顶`的时候，实际上是发送了一个`ajax`请求，地址和数据已经在上图中标出了,
然后会在浏览器写入`Cookie`信息，这样的验证就相当简单了， 我们只要在下次请求的时候不
发送这个cookie就行了。

分析到这人就够了， 接下来我们要实现模拟顶请求。

这里用到一个 `superagent` 的包，安装也很简单。
```
npm i superagent --save
```

<form action="">
    <table>
        <tr>
            <td>评论ID</td>
            <td><input type="text" name="id"></td>
        </tr>
        <tr>
            <td>评论类型</td>
            <td><input type="text" name="id"></td>
        </tr>
        <tr>
            <td>评论次数</td>
            <td><input type="text" name="times"></td>
        </tr>
        <tr>
            <td></td>
            <td><button>提交</button></td>
        </tr>
    </table>
</form>

## 下载

```
/**
 * 蛋疼图刷评脚本
 * author: geer
 * date: 2015-9-10 16:06:58
 * ========================
 * api:
 *     node app.js [comment_id] [type] [times]
 *     说明：
 *         comment_id => 评论ID
 *         type       => 类型 1:顶  2:踩
 *         times      => 刷次数
 * eg:
 *     node app.js 1800231 1 30
 */

var r = require('superagent');

/**
 * 蛋疼图刷赞评论脚本
 * @param  {Number} id   评论ID
 * @param  {Number} type 1:顶 2:踩
 * @param  {Number} times 次数
 * @return 
 */
function comment(id, type, times) {

    if (!times) {
        return;
    }

    r.post('http://www.10danteng.com/qzone/index.php?s=/index/ooxx')
     .set({
        'Accept': '*/*',
        'Accept-Encoding': 'gzip, deflate',
        'Accept-Language': 'zh-CN,zh;q=0.8',
        'Content-Length': '57',
        'Content-Type': 'application/x-www-form-urlencoded',
        'Cookie': 'pgv_pvid=3826611776; PHPSESSID=0bf63e9fa14c3130574f83183f9203ae; D180D81C9A6A00E803EAA06B83A4DC0D=D180D81C9A6A00E803EAA06B83A4DC0D',
        'Host': 'www.10danteng.com',
        'Origin': 'http://www.10danteng.com',
        'Proxy-Connection': 'keep-alive',
        'Referer': 'http://www.10danteng.com/qzone/index.php?s=/index/pages/category_id/1091/id/11669/openid/D180D81C9A6A00E803EAA06B83A4DC0D/openkey/63D75CFD5E08BE31804AABD8C48D88E6/',
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.152 Safari/537.36',
        'X-Requested-With': 'XMLHttpRequest'
     })
     .send({
        type: type,
        id: id,
        openid: 'D180D81C9A6A00E803EAA06B83A4DC0D'
     })
     .end(function (err, res) {
        if (err) {
            console.log(err);
        } else {
            console.log('最新评论数：', res.text);
            // arguments.callee(id, type, --times);
            setTimeout(function() {
                comment(id, type, --times);
            }, 200);
        }
     });

}

if (process.argv.length === 5) {
    comment(process.argv[2], process.argv[3], process.argv[4])
} else {
    console.log('parameters are not match.');
}
```