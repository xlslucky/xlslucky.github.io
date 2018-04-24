[Hexo 博客搭建指南](https://github.com/limedroid/HexoLearning)

[Next主题](http://theme-next.iissnan.com/)
[修改Hexo的Next主题](http://zhouhuix.cn/2016/11/24/%E4%BF%AE%E6%94%B9Hexo%E7%9A%84Next%E4%B8%BB%E9%A2%98/)
[hexo之next主题个性化配置详细教程](http://blog.csdn.net/w_ngzeqi/article/details/73863543)

```shell
# 换了电脑需要安装主题
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```

```shell
# 打包编译
$ hexo clean && hexo g && hexo s

# 提交 (提交之前 必须重新打包编译)
$ hexo d
```

# 其他

## 问题

问题： 安装 hexo-wordcount 报错
解决办法： nodejs7.6之前版本 需要下载hexo-wordcount@2.x版本

问题：上传图片
解决办法：
```shell
# 安装 hexo-asset-image
$ npm install https://github.com/CodeFalling/hexo-asset-image --save

# post_asset_folder: true 设置为true

$ hexo new '<文章名>'

# 会生成和文章名相同的文件夹

# ![name](文章名/xxxx.jpg)
```

问题：在首页摘要里显示文章图片
解决办法：md文章里，使用 <!--more--> 截断
举例：
```shell
![object_prototype_1](prototype/object_prototype_1.png)
<!--more-->
```

问题：换了台电脑，如何继续写文章
解决办法：`_config.yml` 里 `deploy - branch` 设置为 `master`，新建一个分支 提交 没打包之前的代码