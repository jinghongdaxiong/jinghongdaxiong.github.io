---
title: 'mac使用brew update无反应,解决方案更新源'
date: 2019-08-23 14:48:24
tags: [brew,mac]
---

# 原因
  资源访问太慢
  
# 解决方案
更新源

使用中科大的镜像
替换默认源
第一步，替换brew.git

    cd "$(brew --repo)"
    git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
    
第二步：替换homebrew-core.git

    cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
    git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

最后验证

      brew update
    
