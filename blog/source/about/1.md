---
title: hexo-butterfly魔改记录
date: 2022-06-19 21:14:22
tags:
---
### hexo-butterfly魔改记录

使用hexo-butterfly框架搭建个人博客

这里记录一下我自己搭建（魔改/照搬他人）个人博客的步骤，日后查找起来也方便。

欢迎访问我的个人博客点击这里预览效果

留言板信封#
出自https://akilar.top/posts/e2d3c450/

这里直接npm安装配置拿来用了。这里转载一下安装方法：

博客根目录执行

npm install hexo-butterfly-envelope --save
在站点配置文件或者主题配置文件添加配置项（对，两者任一均可。但不要都写）

# envelope_comment
# see https://akilar.top/posts/58900a8/
envelope_comment:
  enable: true #开关
  cover: https://ae01.alicdn.com/kf/U5bb04af32be544c4b41206d9a42fcacfd.jpg #信笺封面图
  message: #信笺内容，支持多行
    - 有什么想问的？
    - 有什么想说的？
    - 有什么想吐槽的？
    - 哪怕是有什么想吃的，都可以告诉我哦~
  bottom: 自动书记人偶竭诚为您服务！ #信笺结束语，只能单行
  height: #调整信笺划出高度，默认1050px
  path: #【可选】comments 的路径名称。默认为 comments，生成的页面为 comments/index.html
  front_matter: #【可选】comments页面的 front_matter 配置
    title: 留言板
    comments: true
字体样式修改#
修改字体样式直接引入css文件和字体包即可。

1、首先寻找喜欢的字体，有些字体很好看并且是免费非商用的，我们可以拿来用。

这里推荐几个网址供参考：方正字库，第一字体网，字体天下，字体家，自由字体

2、将需要使用的字体文件放入博客目录下，我这里是放在blog/source/butterfly/css下，方便css文件引入。

3、如有有css文件，就在最下面继续写；如果没有，则新建一个css文件，文件名任取

@font-face{
  font-family: 'FZFW' ;  /* 自定义字体名称 */
  src: url('FZFWZhu.ttf'); /* 引入字体文件的路径 */
}
/*应用在body体里，放在第一个，font-family会按顺序使用字体族。如果第一个没找到就会找第二个，以此类推。*/
body {
font-family: FZFW,-apple-system, BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", Lato, Roboto, "PingFang SC", "STZhongsong", "Lantinghei SC", sans-serif
}
鼠标样式修改#
这里使用的是小康博客的鼠标样式。如果只想修改鼠标样式，在css文件添加下方代码，然后引入即可。

/*指针样式*/
body {
    cursor: url(https://cdn.jsdelivr.net/gh/sviptzk/HexoStaticFile@latest/Hexo/img/default.cur),
        default;
}
/*链接小手样式*/
a,
img {
    cursor: url(https://cdn.jsdelivr.net/gh/sviptzk/HexoStaticFile@latest/Hexo/img/pointer.cur),
        default;
}
小康博客css修改#
我这里是直接使用大佬现成的文件，主题配置中inject引入css文件。

inject:
  head:
    - <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/static-butterfly/dist/css/index.min.css">
图片透明度修改#
这里使用的是Akilar大佬的修改方案，个人倾向于一图流，比较符合我的审美。

/* 页脚透明 */
#footer{
  background: transparent!important;
}
/* 头图透明 */
#page-header{
  background: transparent!important;
}
/*top-img黑色透明玻璃效果移除，不建议加，除非你执着于完全一图流或者背景图对比色明显 */
#page-header.post-bg:before {
  background-color: transparent!important;
}
/*夜间模式伪类遮罩层透明*/
[data-theme="dark"]
  #footer::before{
      background: transparent!important;
    }
[data-theme="dark"]
  #page-header::before{
    background: transparent!important;
    }
由于前面使用了小康博客的css，页脚的模糊毛玻璃效果没去掉，在页面F12审查元素之后发现是#footer, #footer:before这个选择器有问题，修改如下：

#footer, #footer:before {
    background: transparent!important
}
这样页脚就透明了。

背景音乐添加#
这里参考作者的全局吸底Aplayer教程，为方便后续自己查阅，特摘抄出来。

首先安装hexo-tag-aplayer插件,官方github；

博客根目录安装：

npm install --save hexo-tag-aplayer
由于需要全局都插入aplayer和meting资源，为了防止插入重复的资源，需要把asset_inject设为false

在hexo的配置文件中

aplayer:
  meting: true
  asset_inject: false
在主题配置文件中，enable设为true和per_page设为true

# Inject the css and script (aplayer/meting)
aplayerInject:
  enable: true
  per_page: true
然后把代码插入到页脚中

inject:
  head:
  bottom:
    - <div class="aplayer no-destroy" data-id="000PeZCQ1i4XVs" data-server="tencent" data-type="artist" data-fixed="true" data-mini="true" data-listFolded="false" data-order="random" data-preload="none" data-autoplay="true" muted></div>
运行Hexo就可以看到网页左下角出现了Aplayer

最后，如果你想切换页面时，音乐不会中断。把主题配置文件的pjax设为true即可。

参数解释：

选项	默认值	描述
data-id	必须值	歌曲 id / 播放列表 id / 相册 id / 搜索关键字
data-server	必须值	音乐平台: netease, tencent, kugou, xiami, baidu
data-type	必须值	song, playlist, album, search, artist
data-fixed	false	开启固定模式
data-mini	false	开启迷你模式
data-loop	all	列表循环模式：all, one,none
data-order	list	列表播放模式： list, random
data-volume	0.7	播放器音量
data-lrctype	0	歌词格式类型
data-listfolded	false	指定音乐播放列表是否折叠
data-storagename	metingjs	LocalStorage 中存储播放器设定的键名
data-autoplay	true	自动播放，移动端浏览器暂时不支持此功能
data-mutex	true	该选项开启时，如果同页面有其他 aplayer 播放，该播放器会暂停
data-listmaxheight	340px	播放列表的最大长度
data-preload	auto	音乐文件预载入模式，可选项： none, metadata, auto
data-theme	#ad7a86	播放器风格色彩设置
随机背景图#
参考Akilar大佬的修改方案。

原版的算法生成0的几率很小，所以我把生成随机数的算法改成了Math.floor

首先必须设置网站背景图，在主题配置中找到并且配置。我这里是配置成了'#efefef'

# Website Background (設置網站背景)
# can set it to color or image (可設置圖片 或者 顔色)
# The formal of image: url(http://xxxxxx.com/xxx.jpg)
background: '#efefef'
配置之后，主页使用#web_bg的div来存放背景图片，使用#page-header来存放banner图片。使用js随机选择一张图片，然后根据id取值重设即可做到每次切换或刷新时，随机背景图片。

我这里在blog/source/butterfly/js下新建了rdmbkg.js文件

//随机背景图片数组,图片可以换成图床链接，注意最后一条后面不要有逗号
var backimg =[
  "url(/img/bg1.JPG)",
  "url(/img/bg2.jpg)",
  "url(/img/bg3.jpg)",
  "url(/img/bg4.jpg)"
];
//获取背景图片总数，生成随机数
var bgindex =Math.floor(Math.random() * (backimg.length));
//重设背景图片
document.getElementById("web_bg").style.backgroundImage = backimg[bgindex];
//随机banner数组,图片可以换成图床链接，注意最后一条后面不要有逗号
var bannerimg =[
  "url(/img/bg1.JPG)",
  "url(/img/bg2.jpg)",
  "url(/img/bg3.jpg)",
  "url(/img/bg4.jpg)"
];
//获取banner图片总数，生成随机数
var bannerindex =Math.ceil(Math.random() * (bannerimg.length-1));
//重设banner图片
document.getElementById("page-header").style.backgroundImage = bannerimg[bannerindex];
最后在主题配置文件中引入js

inject:
  head:
  bottom:
    - <script async data-pjax src="/js/randombg.js"></script>
最后在主题配置文件中引入js

inject:
  head:
  bottom:
    - <script async data-pjax src="/js/randombg.js"></script>
侧边栏时钟#
参考这里，我直接npm安装之后拿来用了，下面内容均摘自原博客。

博客根目录执行命令

npm install hexo-butterfly-clock --save
在hexo配置，或者主题配置中任选一个添加如下配置信息即可。二选一，不要都添加。

# electric_clock
# see https://akilar.top/posts/4e39cf4a/
electric_clock:
  enable: true # 开关
  priority: 5 #过滤器优先权
  enable_page: all # 应用页面
  exclude:
    # - /posts/
    # - /about/
  layout: # 挂载容器类型
    type: class
    name: sticky_layout
    index: 0
  loading: /img/loading.gif #加载动画自定义
参数说明

参数	备选值/类型	释义
priority	number	【可选】过滤器优先级，数值越小，执行越早，默认为10，选填
enable	true/false	【必选】控制开关
enable_page	path/all	【可选】填写想要应用的页面的相对路径（即路由地址）,如根目录就填’/‘,分类页面就填’/categories/‘。若要应用于所有页面，就填’all’，默认为all
exclude	path	【可选】填写想要屏蔽的页面，可以多个。写法见示例。原理是将屏蔽项的内容逐个放到当前路径去匹配，若当前路径包含任一屏蔽项，则不会挂载。
layout.type	id/class	【可选】挂载容器类型，填写id或class，不填则默认为id
layout.name	text	【必选】挂载容器名称
layout.index	0和正整数	【可选】前提是layout.type为class，因为同一页面可能有多个class，此项用来确认究竟排在第几个顺位
loading	URL	【可选】电子钟加载动画的图片
添加评论#
最开始使用的是utterances，现在改为Twikoo

具体配置参见：Twikoo

我使用的是Vercel部署，主要是因为免费。作者为我们提供了视频的部署教程，点击查看

我为了偷懒，Vercel使用github登录， MongoDB使用google帐号登录。

补充说明#
这里补充几点：

Vercel添加环境变量MONGODB_URI之后，想要环境变量生效，记得要重启服务（在Deployments里，点击Redeploy）

配置文件的修改在MONGODB控制台中，点击Browse Collections

image

关于代码高亮，需要在配置中配置HIGHLIGHT:true

语法：

<pre><code class="language-css">p { color: red }</code></pre>
其中class="language-css"换成需要的语言。

另外twikoo支持md语法，所以直接使用md语法添加代码块同样可行。

利用企业微信实现twikoo新消息提醒：#
文章参考：

搭建微信通知 API 实现 Twikoo 新消息提醒
微信通知Twikoo新消息提醒（Vercel版本）
微信菌：利用企业微信搭建微信消息提醒API
操作步骤如下：

1 注册企业微信#
进入企业微信创建一个企业。每个普通用户都可以创建企业，不需要很麻烦，但是需要填写一些基本信息。

2 企业微信创建应用#
在 “企业微信 —— 应用管理” 底部的 “自建” 应用处，新建一个 “应用”。
image

创建完成后，记录下应用页面的AgentId和Secret。查看Secret需要安装一个企业微信，查看完可以卸载。

在 “企业微信 —— 我的企业” 底部，记录下 企业 ID。

3 部署 API 云函数#
我之前使用vercel部署过twikoo，因此不需要再创建新的云函数。如果你不是使用vercel，参考这里

找到你的twikoo github仓库，clone到本地，进入api/，执行以下命令：

pip install pipenv
pipenv install requests
然后在该目录创建一个python.py文件，内容如下：

from http.server import BaseHTTPRequestHandler
import json
import requests
from urllib.parse import parse_qs

class handler(BaseHTTPRequestHandler):
    

    def do_GET(self):
        def getTocken(id, secert, msg, agentId):
            url = "https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid=" + \
                id + "&corpsecret=" + secert

            r = requests.get(url)
            tocken_json = json.loads(r.text)
            # print(tocken_json['access_token'])
            sendText(tocken=tocken_json['access_token'], agentId=agentId, msg=msg)

        def sendText(tocken, agentId, msg):
            sendUrl = "https://qyapi.weixin.qq.com/cgi-bin/message/send?access_token=" + tocken
            # print(sendUrl)
            data = json.dumps({
                "safe": 0,
                "touser": "@all",
                "msgtype": "text",
                "agentid": agentId,
                "text": {
                    "content": str(msg)
                }
            })
            requests.post(sendUrl, data)
            
        try:
            params = parse_qs(self.path[12:])
            apiid = params['id'][0]
            apisecert = params['secert'][0]
            apiagentId = params['agentId'][0]
            apimsg = params['msg'][0]
        except:
            apimsg = self.path
        else:
            #try:
            # 执行主程序
            getTocken(id=apiid, secert=apisecert,
                        msg=apimsg, agentId=apiagentId)
            #except:
            #    status = 1
            #    apimsg = '主程序运行时出现错误，请检查参数是否填写正确。详情可以参阅：https://blog.zhheo.com/p/1e9f35bc.html'
            #else:
            #    status = 0
        # print(event)
        # print("Received event: " + json.dumps(event, indent = 2))
        # print("Received context: " + str(context))
        # print("Hello world")

        self.send_response(200)
        self.send_header('Content-type', 'text/plain')
        self.end_headers()
        self.wfile.write(apimsg)
        return
注意文件名命名必须是python.py 否则你可能需要更改获取query parameters部分的代码。

将改动push到远程仓库，等待一会，vercel会自动部署。当Vercel完成部署后，可以使用下面这样的方式，拼接一个 URL，浏览器访问，看看手机微信能不能接收到消息。

API参数：

参数	类型	必选	描述	示例
id	str	true	企业微信公司id	ww42a2d7**********
secert	str	true	企业微信应用的应用secert	xD_*****_6hVymgTBZuTaZviu9i3P4Xd6**********
agentId	int	true	企业微信应用的应用agentId	1000003
msg	str	true	需要发送的内容	helloworld
完整的url如下所示：

https://<vercel_app_address>/api/python?id=<企业id>&secert=<secret>&agentId=<agentId>&msg=测试一下吧
将上述url的内容替换成你的信息。其中，vercel_app_address可以在vercel应用界面查看，如下图：
image

测试一下你的访问路径是否有效，如果能收到消息就说明成功。

4 在twikoo中配置#
在twikoo后台管理WECOM_API_URL中添加你拼接的url即可。

注意msg后面不要有参数：

https://<vercel_app_address>/api/python?id=<企业id>&secert=<secret>&agentId=<agentId>&msg=
image

5 在微信中接收企业微信消息#
在“企业微信——我的企业——微信插件”页面配置，点击这里查看。

使用微信扫码，关注你的企业微信，并且在设置中打开允许成员在微信插件中接收和回复聊天消息选项。

image

大功告成！现在，使用一个非博主的邮箱，去评论一条试试吧。

搜索功能添加#
参考这里，我只添加了本地搜索。

根目录输入命令安装

npm install hexo-generator-search --save
然后在hexo的配置文件中添加

search:
  path: search.xml
  field: post
  content: true
  template: 
在主题配置文件中打开本地搜索

# Local search
local_search:
  enable: true
页脚徽标#
参考这里，通过shields.io在线生成，

label: 标签，徽标左侧内容
message: 信息，徽标右侧内容
color: 色值，支持支持十六进制、rgb、rgba、hsl、hsla 和 css 命名颜色。十六进制记得删除前面的 #号
输入相关信息后，点击 make badge 即可得到徽标的 URL。可以用 img 标签引用，写法简单。不过正式写法建议用 object 标签引用，写法示例如下。

<!-- label=Frame，Message=Hexo，color=blue -->
https://img.shields.io/badge/Frame-Hexo-blue
<!-- 在页面上可以使用img标签来引用 -->
<img src="https://img.shields.io/badge/Frame-Hexo-blue">
<!-- 部分属性例如link需要用object标签来引用 -->
<object data="https://img.shields.io/badge/Frame-Hexo-blue?link=https://hexo.io"></object>
还可以继续添加参数，在 URL 内添加样式属性，在url后面使用 ? 引用，使用 & 连接各属性。

属性	说明	示例
style	徽标样式，默认提供了五种样式： plastic,flat,flat-square, for-the-badge,social	?style=flat-square
label	覆盖默认的左侧文本 （空格或特殊字符需要转 URL 编码！）	?label=healthinesses
logo	给左侧标签前插入图标 可以访问 simpleicons 查询图标	?logo=Hexo
logo	自定义图标， 限制较多，不推荐	?logo=data:image/png;base64,url
logoColor	设置徽标的颜色 （支持十六进制、rgb、 rgba、hsl、hsla 和 css 命名颜色）。 支持命名徽标， 但不支持自定义徽标。	?logoColor=violet
logoWidth	给图标提供的水平空间	?logoWidth=40
link	徽标指向的链接， 此时需要用 object 标签 引用才能生效 写法看示例代码	?link=http://example.com
labelColor	左侧部分背景色， （支持十六进制、rgb、 rgba、hsl、hsla 和 css 命名颜色）	?labelColor=abcdef 或者？colorA=abcdef
color	右侧部分背景色， （支持十六进制、rgb、 rgba、hsl、hsla 和 css 命名颜色）	?color=fedcba 或者？colorB=fedcba
在 [Blogroot]\_config.butterfly.yml 的 footer 配置项中添加徽标，注意事先压缩一下，使他们只留一行。为了不至于太过紧凑，设置一下行内间隔属性 margin-inline。

      footer:
        owner:
          enable: true
          since: 2016
-       custom_text:
+       custom_text: <p><a style="margin-inline:5px" target="_blank" href="https://hexo.io/"><img src="https://img.shields.io/badge/Frame-Hexo-blue?style=flat&logo=hexo" title="博客框架为Hexo"></a><a style="margin-inline:5px" target="_blank" href="https://butterfly.js.org/"><img src="https://img.shields.io/badge/Theme-Butterfly-6513df?style=flat&logo=bitdefender" title="主题采用butterfly"></a><a style="margin-inline:5px" target="_blank" href="https://www.jsdelivr.com/"><img src="https://img.shields.io/badge/CDN-jsDelivr-orange?style=flat&logo=jsDelivr" title="本站使用JsDelivr为静态资源提供CDN加速"></a><a style="margin-inline:5px" target="_blank" href="https://vercel.com/ "><img src="https://img.shields.io/badge/Hosted-Vercel-brightgreen?style=flat&logo=Vercel" title="本站采用双线部署，默认线路托管于Vercel"></a><a style="margin-inline:5px" target="_blank" href="https://vercel.com/ "><img src="https://img.shields.io/badge/Hosted-Coding-0cedbe?style=flat&logo=Codio" title="本站采用双线部署，联通线路托管于Coding"></a><a style="margin-inline:5px" target="_blank" href="https://github.com/"><img src="https://img.shields.io/badge/Source-Github-d021d6?style=flat&logo=GitHub" title="本站项目由Gtihub托管"></a><a style="margin-inline:5px" target="_blank" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img src="https://img.shields.io/badge/Copyright-BY--NC--SA%204.0-d42328?style=flat&logo=Claris" title="本站采用知识共享署名-非商业性使用-相同方式共享4.0国际许可协议进行许可"></a></p>
        copyright: false # Copyright of theme and framework
        ICP: # Chinese ICP License
ICP备案的徽标可以通过logo属性添加，连接如下

https://img.shields.io/badge/%E6%B5%99ICP%E5%A4%87-20210819%E5%8F%B7--1-e1d492?style=flat&logo=data:image/png;base64,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
SEO优化#
1 文章路径#
做了一些文章路径的优化，Hexo默认永久链接是 :year/:month/:day/:title/的格式，这样不美观，也不利于SEO。

在站点配置文件中修改：

permalink: posts/:hash/	 # 我这里改成posts/:hash
这样文章的路径就没有讨厌的年月日格式了。

2 添加站点地图#
我这里使用的是hexo-generator-seo-friendly-sitemap，生成网站地图。

网站地图是什么？

网站地图实际上就像是一个站点的导航文件。网站地图的重要性：

搜索引擎每天都是让爬虫在互联网爬行来抓取页面，站点地图的作用就是给爬虫爬行构造了一个方便快捷的通道，因为网站页面是一层一层的链接的，其中可能会存在死链接的情况，如果没有站点地图，爬虫爬行在某个页面就因死链接爬行不了，那么就不能收录那些断链接的页面。

站点地图的存在不仅是满足搜索引擎爬虫的查看，更多是方便网站访客来浏览网站，特别是例如门户型网站由于信息量太多很多访客都是通过站点地图来寻找到自己需要的信息页面，这也能很好的提高用户体验度 。

站点地图可以提高链接页面的权重，因为站点地图是指向其他页面的链接，此时站点地图就给页面增加了导入链接，大家知道导入链接的增加会影响到页面的权重，从而提高页面的权重，页面权重的提高同时会提高页面的收录率。

使用方法：

hexo根目录下安装：

npm install hexo-generator-seo-friendly-sitemap --save
在Hexo站点配置文件添加：

sitemap:
    path: sitemap.xml
    tag: false
    category: false
参数	解释
path	索引地图的路径，保持默认就好
tag	false:标签页不添加到网站地图中（推荐）
category	false:分类页不添加到网站地图中（推荐）
设置之后，网站地图就生成完毕了。

以我的站点为例，

网站地图索引：https://www.yyyzyyyz.cn/sitemap.xml

文章网站地图：https://www.yyyzyyyz.cn/post-sitemap.xml

页面网站地图：https://www.yyyzyyyz.cn/page-sitemap.xml

接下来，我们需要将生成的网站地图提交到谷歌、百度、必应等站点，注册账号，添加你的域名，然后复制刚才生成的网站地图上传。之后等待爬虫抓取就好了。下面是sitemap上传示例。

百度：



必应：



谷歌：

关于谷歌，有能力的同学可以尝试一下。



3 自动推送#
使用hexo-submit-urls-to-search-engine插件，每次hexo -d时，可自动推送Hexo博客新链接至谷歌、百度、必应搜索引擎站长平台以提升网站收录质量和速度。解放双手，一劳永逸。

首先在本地hexo根目录下安装：

npm install --save hexo-submit-urls-to-search-engine
获取站长平台API token，关于这部分内容，官方文档有详细介绍，点击查看

假设你已经阅读了官方文档，并且获得了token。获取token之后，Hexo站点配置文件添加：

hexo_submit_urls_to_search_engine:
  submit_condition: count #链接被提交的条件，可选值：count | period 现仅支持count
  count: 10 # 提交最新的10个链接
  period: 900 # 提交修改时间在 900 秒内的链接
  google: 0 # 是否向Google提交，可选值：1 | 0（0：否；1：是）
  bing: 1 # 是否向bing提交，可选值：1 | 0（0：否；1：是）
  baidu: 1 # 是否向baidu提交，可选值：1 | 0（0：否；1：是）
  txt_path: submit_urls.txt ## 文本文档名， 需要推送的链接会保存在此文本文档里
  baidu_host: https://cjh0613.github.io ## 在百度站长平台中注册的域名
  baidu_token: 请按照文档说明获取 ## 请注意这是您的秘钥， 所以请不要把它直接发布在公众仓库里!
  bing_host: https://cjh0613.github.io ## 在bing站长平台中注册的域名
  bing_token: 请按照文档说明获取 ## 请注意这是您的秘钥， 所以请不要把它直接发布在公众仓库里!
  google_host: https://cjh0613.github.io ## 在google站长平台中注册的域名
  google_key_file: Project.json #存放google key的json文件，放于网站根目录（与hexo _config.yml文件位置相同），请不要把json文件内容直接发布在公众仓库里!
  google_proxy: http://127.0.0.1:8080 # 向谷歌提交网址所使用的系统 http 代理，填 0 不使用
  replace: 0  # 是否替换链接中的部分字符串，可选值：1 | 0（0：否；1：是）
  find_what: 
  replace_with: 
关于谷歌，如果不想提交到谷歌，设置google和google_proxy为0。

配置完成。

接下来只需要hexo clean && hexo generate && hexo deploy即可。

如果推送成功，你会看到如下消息：

Bing response:  { d: null }
Bing response:  { d: null }
Bing response:  { d: null }
Bing response:  { d: null }
Bing response:  { d: null }
Bing response:  { d: null }
Bing response:  { d: null }
Bing response:  { d: null }
Baidu response:  {"remain":2992,"success":8}
Google response:  {
  urlNotificationMetadata: {
    url: 'https://www.yyyzyyyz.cn/posts/25075e302733/',
    latestUpdate: {
      url: 'https://www.yyyzyyyz.cn/posts/25075e302733/',
      type: 'URL_UPDATED',
      notifyTime: '2021-11-18T11:16:27.108920085Z'
    }
  }
}
4 添加robots.txt#
关于robots协议，可以查看我的这篇博客，简单来说就是可以指定搜索引擎爬虫可以抓取什么内容、不可以抓取什么内容。

这些网站可以在线生成robots.txt任选一个即可：tool在线生成，ChinaZ在线生成，dqdv在线生成，w3cschool在线生成

复制生成的内容，新建一个robots.txt将内容粘贴进去，然后将它上传至网站根目录下。

可以在这里验证你的文件是否生效：验证robots

5 添加rel#
为网站使用到的所有外链添加rel="noopener external nofollow noreferrer", 可以有效地加强网站SEO和防止权重流失。github

hexo博客根目录安装：

npm i hexo-filter-nofollow --save
然后在配置文件添加：

nofollow:
  enable: true
  field: site
使用Github Action自动部署博客#
参考这篇文章。

首先，要保证使用git安装了butterfly主题而不是npm安装，因为通过npm安装并不会在themes里生成主题文件夹，而是在 node_modules里生成。

操作步骤如下：

1 获取token#
登录GitHub，访问 Github-> 头像（右上角）->Settings->Developer Settings->Personal access tokens（点击跳转），点击generate new token，创建token，名称随意，需要勾选repo权限。

复制这个token备用，token只会显示这一次，如果忘记需要重新生成。

2 创建存放源码的私有仓库#
我们需要创建一个用来存放 Hexo 博客源码的私有仓库，由于仓库会保存刚刚生成的token，如果泄露会导致安全问题，因此选择闭源。用[SourceRepo]表示。

3 创建另一个仓库存放编译好的博客文件#
这个仓库可以公开，也可以私有。用[GithubBlogRepo]表示。

4 配置 deploy 项#
打开站点配置文件 博客根目录/_config.yml, 找到 deploy 配置项，使用之前生成的token和github仓库 URL 来组装地址。

deploy:
- type: git
  repo:
    gitHub: https://GitHub用户名:刚刚生成的token@github.com/Github用户名/[GithubBlogRepo].git,master
5 配置 Github Action#
在博客根目录新建.github 文件夹，注意开头是有个. 。然后在.github 内新建 workflows 文件夹，再在 workflows 文件夹内新建 autodeploy.yml, 在 [Blogroot]/.github/workflows/autodeploy.yml 里面输入如下内容：

# 当有改动推送到master分支时，启动Action
name: 自动部署

on:
  push:
    branches:
      - master

  release:
    types:
      - published

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: 检查分支
      uses: actions/checkout@v2
      with:
        ref: master

    - name: 安装 Node
      uses: actions/setup-node@v1
      with:
        node-version: "12.x"

    - name: 安装 Hexo
      run: |
        export TZ='Asia/Shanghai'
        npm install hexo-cli -g

    - name: 缓存 Hexo
      uses: actions/cache@v1
      id: cache
      with:
        path: node_modules
        key: ${{runner.OS}}-${{hashFiles('**/package-lock.json')}}

    - name: 安装依赖
      if: steps.cache.outputs.cache-hit != 'true'
      run: |
        npm install --save

    - name: 生成静态文件
      run: |
        hexo clean
        hexo generate

    - name: 服务器验证
      env:
        ACTION_DEPLOY_KEY: ${{ secrets.HEXO_DEPLOY_PRI }}
      run: |
        sudo timedatectl set-timezone "Asia/Shanghai"
        mkdir -p ~/.ssh/
        echo "$ACTION_DEPLOY_KEY" > ~/.ssh/id_rsa
        chmod 600 ~/.ssh/id_rsa
        ssh-keyscan 10.**.5*.*** >> ~/.ssh/known_hosts  #此处填写你的服务器IP
        git config --global user.name "yyyz"  #修改为你的用户名
        git config --global user.email "*********@qq.com" #修改为你的GitHub用户名邮箱
        
    - name: 部署
      run: |
        git config --global user.name "yyyz"  #修改为你的用户名
        git config --global user.email "*********@qq.com" #修改为你的GitHub用户名邮箱
        git clone https://GitHub用户名:刚刚生成的token@github.com/Github用户名/[GithubBlogRepo].git .deploy_git
        # 此处务必用HTTPS链接。SSH链接可能有权限报错的隐患
        # =====注意.deploy_git前面有个空格=====
        # 这行指令的目的是clone博客静态文件仓库，防止Hexo推送时覆盖整个静态文件仓库，而是只推送有更改的文件
        hexo deploy
6 在博客源码仓库添加secrets#
我们需要在自动部署时使用免密登录，因此要在刚刚创建的存放源码的私有仓库[SourceRepo]内添加一个secrets。

进入私有仓库点击settings->secrets->new repository secret，其中Name为HEXO_DEPLOY_PRI；Value为你本地电脑上.ssh文件夹下为服务器生成的免密登录的私钥，我这里是id_rsa，复制私钥内容粘贴即可。

7 重新设置远程仓库和分支#
博客根目录下，添加.gitignore文件：

.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
.deploy_git*/
.idea
themes/butterfly/.git
在博客根目录下启动终端，使用 git 指令重设仓库地址。

git remote rm origin # 删除原有仓库链接
git remote add origin git@github.com:Github用户名/[SourceRepo].git #[SourceRepo]为新的存放源码的github私有仓库
git add .
git commit -m "github action update"
git push origin master
前往源码仓库查看，如果你发现theme/butterfly文件打不开并且有一个箭头，是由于我们clone安装的butterfly主题，这个文件夹里面有.git隐藏文件，github就将他视为一个子系统模块了。

解决办法就是：

删除文件夹里面的.git文件夹
执行git rm --cached theme/butterfly
然后重新add、commit、push即可。

打开 GIthub 存放源码的私有仓库，找到 action，查看是否成功上传博客。

这种方式有一个缺点是每次更新时，你所有的文章更新时间都会改变，于是我干脆把更新时间隐藏了。