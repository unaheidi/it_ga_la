Jenkins环境的复制     
===============
背景：

* 公司几十号 ios 开发人员几年前在 Mac mini 上安装了 jenkins，现在 ios 开发的工作量在加大，急需提高 jenkins 服务器的性能
* 研发负责人提出新购置 Mac Pro 来替代 Mac mini，让工具工程师在一周内把老设备上的环境迁到新设备上。

工具工程师技能：

* 熟悉 Windows/Centos/Ubuntu 操作系统，没有接触过 OS X Yosemite
* 熟悉 jenkins
* 没接触过 ios 开发流程

人员配置：

* 一名工具工程师，全程负责 jenkins 新环境的搭建
* 一名 ios 开发人员，负责苹果开发要求的证书从老机器复制到新机器（兼职）
* 业务线开发人员，数枚，能不打扰就不打扰

本文站在工具工程师的视角，介绍整个复制的步骤，对易出错或缺少参考资料的几个环节进行详细的描述，并提供解决方法。

#### 复制步骤

1. 在 Mac Pro 上用 pkg 包安装 jenkins。
2. 把 Mac Mini 上所有的 job 用 tar 命令打包后，拷贝到 Mac Pro 上，然后解压。
3. 参考 Mac Mini 上 jenkins 的配置文件，把和 view 相关的内容复制到 Mac Pro 的 jenkins 的配置文件中。
4. 在 Mac Mini 上找到苹果开发需要的证书，把这些证书拷贝后导入到 Mac Pro 中。
5. 打开 Xcode 运行 jenkins job 中 的 project，保证通过。
6. 打开 jenkins 的 job，使其通过 build。

#### 主要的问题

1. jenkins 运行 job 时，好多命令不能使用
2. jenkins 运行 job 时，报 “User interaction is not allowed.” 错误。
3. 重启 OS 后，jenkins 服务没运行。

#### 解决方法
1. jenkins 运行 job 时，好多命令不能使用。解决方法：

    用 pkg 包安装的 jenkins，其服务是用 jenkins
    帐号运行的，而安装证书、安装 CocoaPods 等管理活动用的是另一个具有 sudo 权限的帐号（admin）。于是，把 jenkins 相关的文件权限从 jenkins 改变为 admin，同时，修改 /Library/LaunchDaemons/org.jenkins-ci.plist 文件，把 UserName 从 jenins 改为 admin 。

2. jenkins 运行 job 时，报 “ User interaction is not allowed. ” 错误。解决方法：
    
    这个错误发生在 codesign 的环节，在 google 上搜了很久中英文资料，最后我是这么解决的：
    
    * 把 org.jenkins-ci.plist 文件从 /Library/LaunchDaemons/ 移到 /Library/LaunchAgents/ 路径下。
    * 把 `security unlock-keychain -p "passwd" "/Users/admin/Library/Keychains/login.keychain"` 这句话写到 `/Library/Application Support/Jenkins/jenkins-runner.sh` 这个文件中。
    
    这样处理的好处在于：确保重新启动 jenkins 服务或者重启机器后，能完成 codesign，同时还不用单独在每个 job 中执行 unlock-keychain 。

3. 重启 OS 后，jenkins 服务没运行。
    
    看 jenkins 日志发现系统调用了 /Library/LaunchAgents/org.jenkins-ci.plist，jenkins 运行了一会儿马上又停掉了。通过 google 查找到方法，把 admin 帐号设置为自动登录，然后重启机器，此时，jenkins服务便正常运行了。

#### 不足

1. 能解决问题，为什么这么解决，没有彻底搞清楚各个环节。
2. 证书的导入是由 ios 工程师完成的，我作为工具工程师不清楚该怎么做。

#### 参考资料

* 提到了 LaunchAgents 和 LaunchDaemons 的区别。
[jenkins_keychain_timeouts][jenkins_keychain_timeouts]
* 提到把用户设置为 auto-login 就能启动 jenkins 服务。
[unable-to-sign-ios-builds-with-jenkins][unable-to-sign-ios-builds-with-jenkins]

[jenkins_keychain_timeouts]: http://modeset.com/what-we-know/2013/03/11/jenkins_keychain_timeouts
[unable-to-sign-ios-builds-with-jenkins]: http://stackoverflow.com/questions/9626447/unable-to-sign-ios-builds-with-jenkins








