---
title: hexo使用说明
categories: interview #分类
tags: [java,study,interview] #文章标签，可空，多标签请用格式，注意冒号:后面有个空格
top_img: image/13.jpeg
cover: image/13.jpeg
---

1、默认安装
npx hexo init blog
cd blog
npm install
npx hexo server

2、修改主题
2.1删除原来默认主题
rm -rf theme
2.2下载你喜欢的主题
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
2.3修改主题配置
vi _config
theme:butterfly

2.4这个主题需要安装插件
npm install hexo-renderer-pug hexo-renderer-stylus
2.5启动
npx hexo server

3、publish到github
npx hexo clean
npx hexo g
npx hexo d

4、github设置站点
https://docs.github.com/cn/pages




