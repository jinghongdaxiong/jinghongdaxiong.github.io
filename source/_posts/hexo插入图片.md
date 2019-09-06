---
title: hexo插入图片
date: 2019-09-06 10:26:13
tags: [hexo]
categories: [hexo]
---
* 当Hexo项目中只用到少量图片时，可以将图片统一放在source/images文件夹中，通过markdown语法访问它们。
  
  ![](/images/image.jpg)

* 图片除了可以放在统一的images文件夹中，还可以放在文章自己的目录中。文章的目录可以通过配置_config.yml来生成。


    post_asset_folder: true

将_config.yml文件中的配置项post_asset_folder设为true后，执行命令$ hexo new post_name，在source/_posts中会生成文章post_name.md和同名文件夹post_name。将图片资源放在post_name中，文章就可以使用相对路径引用图片资源了。

    _posts/post_name/image.jpg
    1
    ![](image.jpg)
    

[参考] (https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/)