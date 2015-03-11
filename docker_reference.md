Docker参考   
===============

#### 中英文资料

* 官方指南
[dock官方指南] [官方指南]
* 深入浅出Docker
[深入浅出Docker] [深入浅出Docker] 
* 看服务器是否为docker虚机
[是否为docker虚机][是否为docker虚机]

[官方指南]: https://docs.docker.com/userguide/
[深入浅出Docker]: http://www.infoq.com/cn/articles/docker-core-technology-preview
[是否为docker虚机]: http://tuhrig.de/how-to-know-you-are-inside-a-docker-container/

#### docker常用命令

* 根据当前路径下的dockerfile，创建image  
  *docker build -t "alpha_machine:v0.7" .*
* 根据image创建虚机，为虚机分配ip  
  *docker run -P -d --ip="192.168.218.224/24@192.168.218.1" alpha_machine:v0.7*
* 根据修改后的container创建image  
  *docker commit -m='Push to docker.diors.it' -a='ezc' c07f6b925d76 docker.diors.it/alpha_machine:v0.8*
* 从中央仓库拉image  
  *docker pull docker.diors.it/alpha_machine:v0.8*
* 为image创建tag
  *docker tag 93c447941695 docker.diors.it/beta_machine:v1.0*
* 查看所有的image   
  *docker images*
* 查看所有虚机   
  *docker ps -a*
* 查看运行着的虚机   
  *docker ps*
* 启动虚机   
  *docker start c07f6b925d76*
* 停止虚机   
  *docker stop c07f6b925d76*
* 删除未运行的虚机   
  *docker rm c07f6b925d76*
* 删除运行中的虚机   
 *docker rm -f c07f6b925d76*
* 启动所有的虚机   
 *docker start $(docker ps -a -q)*





