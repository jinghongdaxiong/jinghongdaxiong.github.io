---
title: Vagrant基本用法
date: 2018-09-11 16:06:18
tags: vagrant
categories: 
- 虚拟机
---

## 官网: https://www.vagrantup.com/

# intro 介绍

## what is vagrant (流浪汉是什么)

### Introduction to Vagrant (流浪汉简介)

    Vagrant is a tool for building and managing virtual machine
    environments in a single workflow. With an easy-to-use workflow
    and focus on automaiton, Vagrant lowers development setup time,
    increases production parity,and makes the "works on my machine"
    excuse a relic of the past.
    
    if you are already familiar with the basics of Vagrant, the 
    documentation provides a better reference build for all available
    features and internals.
    
    Vagrant是一种在单个工作流程中构建和管理虚拟环境的工具.通过易于使用的工
    作流程并专注于自动化,Vagrant降低了开发环境的设置时间,提高了生产平价,
    并使"在我的假期上工作"称为过去的遗留物.
    
    如果您已经熟悉Vagrant的基础知识,那么该文档可为所有可用功能和内部提供
    更好的参考构建.
    
### why Vagrant (为什么选择Vagrant)    

    vagrant provides easy to configure,reproducible,and portable
    work environments built on top of industry-standard technology
    and controlled by a single consistent workflow to help maximize 
    the productivity and flexibility of you and your team.
    
    To achieve its magic,Vagrant stands on the shoulders of giants.
    Machines are provisioned on top of VirtualBox,VMware,AWS,or any
    other provider,Then,Industry-standard providioning tools such
    as shell scripts,Chef,or Puppet,can automatically install and
    configure software on the virtual machine.
    
    Vagrant 提供易于配置,可重复和便携的工作环境,基于行和标准技术构建,并由单一
    一致的工作流程控制,以帮助您和您的团队最大限度地提高生成力和灵魂性.
    
    为了实现其魔力,Vagrant站在巨人的肩膀上.在VirtualBox,VMware,AWS或
    任何其他提供商之上配置计算机.然后,行业标准配置工具(如shell脚本,Chef或
    Puppet)可以在虚拟机上自动安装和配置软件.
    
### For Developers (对于开发者)

    if you are a developer,Vagarnt will isolate dependencies and
    their configuration within a single disposable,consistent 
    environment,without sacrificing any of the tools you are
    used to working with(editors,browsers,debuggers,etc.).Once
    you or someone else creates a single Vagrantfile,you just 
    need to vagrant up and everything is installed and configured
    for you to work .Other members of your team create their
    development environments form the same configuration,so 
    whether you are working on Linux,Mac OSX,or Windows,all
    your team members are running code in the same environment
    ,against the same dependencies,al configured the same way.
    Say goodbye to "works on my machine"bugs.
    
    如果您是一位开发人员,Vagrant将在一个一致的环境中隔离依赖及其配置,
    而不会牺牲您习惯使用的任何工具(编辑器,浏览器,调试器等).一旦您或其他人
    创建了单个Vagrant文件,您只需要运行vagrant up命令安装并配置所有内容
    即可使用.团队的其他成员使用相同的配置创建他们的开发环境,因此无论您是在
    Linux,Mac OS X还是Windows上工作,您的所有团队成员都在同一环境中运行
    代码,针对相同的依赖项,所有组件都配置相同办法.告别"在我的机器上工作"的
    错误.
    
### For Operators (对于运维)
    
    if you are an operations engineer or DevOps engineer,Vagrant
    gives you a disposable environment and consisitent workflow
    for developing and testing infrastructure management scripts.
    you can quickly test things like shell scripts,Chef cookbooks,
    Puppet modules,and more using local virtualization such as 
    VirtualBox or VMware.Then,with the same configuration,you can
    test these scripts on remote clouds such as AWS or RackSpace 
    with the same workflow.Ditch your custom scripts to recycle
    EC2 instances,stop juggling SSH prompts to various machines,
    and start using Vagrant to bring sanity to your life.
    
    如果你是运维工程师或者开发运维工程师,Vagrant为您提供一次性环境和一致的
    工作流程,用于开发和测试基础架构管理脚本.您可以使用VirtualBox或VMware
    等本地虚拟化快速测试shell脚本,Chef cookbook,Puppet模块等内容.然后
    ,使用相同的配置,您可以使用相同的工作流在远程云(如AWS或RackSpace)上测试
    这些脚本.抛弃自定义脚本以回收EC2实例,停止将SSH提示传递给各种计算机,并
    开始使用Vagrant为您的生活带来理智.
    
### For Designers(对于设计)

    If you are a designer,Vagrant will automatically set everything
    up that is required for that web app in order for you to focus
    on doing what you do best:design.Once a developer configures
    Vagrant,you do not need to worry about how to get that app running
    ever again.No more bothering other developers to help you fix
    your environment so you can test designs.Just check out the code
    ,vagrant up,and start designing.
    
    如果你是个设计师,Vagrant 将自动设置该web应用程序所需的所有内容,以便你
    集中精力做你最擅长的事情:设计.当开发人员配置了一次Vagrant,你再也不用
    担心怎样去运行应用程序.不必再打扰其他开发人员来帮助你修复环境,因此你能
    测试你的设计.仅打出单词,vagrant up ,就可以开始设计了.
    
    
### For Everyone(对于任何人)         
    
    Vagrant is designed for erveyone as the easiest and fastest
    way to create a virtualized environment!
    
    Vagrant 的设计对任何人来说都是最简单最快速的获取虚拟环境的方式.

## Vagrant vs. Other Software (Vagrant 与其他软件的对比)    

    Vagrant is not the only tool to manage virtual machines
    and development environments.This section compares Vagrant
    to these other software choices.
    
    Vagrant 不是唯一管理虚拟机和开发环境的工具,本节将比较Vagrant与其他
    软件以便选择.
    
    Due to the bias of the comparisons,we attempt to only use
    facts.If you find something that is invalid or out of date
    in the comparisons,please open an issue and we'll address
    it as soon as possible.
    
    由于比较的偏颇,我们试图只用事实说话,在比较中假如你发现了无效的或者过期
    的内容,请开启一个issue,我们将尽可能的处理它.
    
    Use the navigation on the left to read comparisons of Vagrant
    versus similar software.
    
    使用左侧导航栏去阅读Vagrant与相似软件的比较
    
###   CLI Tools
      
#### Vagrant vs CLI Tools

    Virtualization software like VirtualBox and VMware come with
    command line utilities for managing the lifecycle of machines
    on their platform.Many people make use of these utilties for 
    managing the lifecycle of machines on their platform.Many people
    make use of these utilities to write their own automation.
    Vagrant actually uses many of these utilties internally.
    
    虚拟化软件如VirtualBox和VMware,带有命令行工具来管理平台上的机器的生命
    周期.许多人使用这些工具来写他们自己的自动化程序.Vagrant实际上在内部用了
    很多这样的工具.
    
    The difference between these CLI tool and Vagrant is that 
    Vagrant builds on top of these utilties in a number of ways 
    while still providing a consistent workflow.Vagrant supports
    multiple provisioners to setup the machine,automatic SSH setup,
    creating HTTP tunnels into your development environment,and more.
    All of these can be configured using a single simple configuration
    file.
    
    这些CLI工具与Vagrant的区别在于,Vagrant以多种方式在这些实用程序之上
    构建,同时仍然提供一致的工作流.Vagrant 支持多种同步文件夹类型,设置机器
    的多个提供程序 自动SSH设置 创建多开发环境中的HTTP隧道等.所有这些都可以
    使用一个简单的配置文件来配置.
    
    Vagrant still has a number of improvements over manual scripting
    even if you ignore all the higher-level features Vagrant provides.
    The command-line utilities provided by vitualization software
    often change each version or have subtle bugs with workarounds.
    Vagrant automatically detects the version,uses the correct flags,
    can work around known issues.So if you're using one version
    of VirtualBox and a co-worker is using a different version,
    Vagrant will still work consistently.
    
    Vagrant在手动脚本方面仍有许多改进.即使你忽略了流浪汉提供的所有高级特征.
    虚拟化软件提供的命令行工具经常改变每个版本或有变通方法的细微错误.
    流浪汉自动检测版本,使用正确的标志,能围绕已知问题工作.
    因此假如你使用一个版本的VirtualBox而另一个同事使用不同的版本,Vagrant
    仍然可以一致工作.
    
    For highly-specifil workflows that don't change often.it can
    still be beneficial to maintain custom scripts .Vagrant is 
    tageted at building development environments but some 
    advanced users still use the ClI tools underneath to do 
    other manual things.
    
    对于不经常改变的高度特定的工作流,维护自定义脚本仍然是有益的.Vagrant
    的目标是构建开发环境,但是一些高级用户仍然使用下面的CLI工具来完成
    其他手动操作.
    
###     Docker

#### Vagrant vs. Docker

    Vagrant is a tool focused on providing a consistent
    development environment workflow across multiple 
    operating systems.Docker is a container management
    that can consistently run sofeware as long as a 
    containerization system exists.
    
    Vagrant是一个专注于跨多个操作系统提供一致的开发环境工作流的工具.
    Docker是一种容器管理,只要存在容器化系统,就可以始终如一地运行软件.
    
    Containers are generally more lightweight than virtual
    machines,so starting and stopping containers is extremely
    fast. Most common development machines don't have a 
    containerization system built-in, and Docker uses a 
    virtual machine with Linux installed to provide that.
    
    容器一般比虚拟机更轻量级,因此开启或停止容器是非常快的.大多数常见的
    开发及其没有内置化的容器系统,Doker使用安装了Linux的虚拟机来提供这
    一点.
    
    Currently,Docker lacks support for certain operating
    systems(such as BSD).if you target deployment is one
    of these operating systems,Docker will not provide 
    the same prodution parity as tool like Vagrant.Vagrant 
    will allow you to run a Windows development environment
    on Mac or Linux,as well.
    
    目前,Docker缺乏对某些操作系统(如:BSD)的支持.如果你的目标是部署
    这些操作系统之一,那么Docker不会像Vagrant一样提供相同的一致产品.
    Vagrant允许你在Mac或Linux上运行Windows开发环境.
    
    For microservice heavy environments,Docker can be attractive
    because you can easily start a single Docker VM and start
    many containers above that very quickly.This is a good use
    case for Docker.Vagrant can do this as well with the Docker
    provider.A primary benefit for Vagrant is a consistent
    workflow but there are many cases where a pure-Docker
    workflow does make sense.
    
    Both Vagrant and Docker have a vast library of 
    community-contributed "images"or"boxes"to choose from.
    
    对于微服务繁重的环境,Docker可能很有吸引力,因为您可以单个Docker
    VM 并快速启动多个容器.这是Docker的一个很好的用例.Vagrant也可以
    使用Docker提供程序执行此操作.Vagrant的主要好处是一致的工作流程,
    但在很多情况下,纯Docker工作流程确实有意义.
    
    Vagrant和Docker都拥有庞大的社区贡献"图像"或"盒子"库供您选择.
    
### Terraform

#### Vagrant vs. Terraform

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    


