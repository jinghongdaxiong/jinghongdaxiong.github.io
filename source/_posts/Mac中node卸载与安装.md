---
title: Mac中node卸载与安装
date: 2019-08-23 14:35:56
tags: [node,npm,brew]
---

## 卸载

在终端依次输入以下命令

    sudo npm uninstall npm -g
     
     sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*
     
     sudo rm -rf /usr/local/include/node /Users/$USER/.npm
     
     sudo rm /usr/local/bin/node
     
     sudo rm /usr/local/share/man/man1/node.1
    
     sudo rm /usr/local/lib/dtrace/node.d
     
## 验证是否成功
    
    node -v  //not found
    
    npm -v //not found
    
## 安装
    
    brew search node
    brew install node
             
## 遇到的坑             

    Error: The `brew link` step did not complete successfully
    The formula built, but is not symlinked into /usr/local
    Could not symlink share/doc/node/lldb_commands.py
    Target /usr/local/share/doc/node/lldb_commands.py
    already exists. You may want to remove it:
      rm '/usr/local/share/doc/node/lldb_commands.py'
    
    To force the link and overwrite all conflicting files:
      brew link --overwrite node
    
    To list all files that would be deleted:
      brew link --overwrite --dry-run node

## 解决办法

      rm '/usr/local/share/doc/node/lldb_commands.py'

      brew link --overwrite node
      
      
