# docker k8s mesos study

CentOS系统更换软件安装源

第一步：备份你的原镜像文件，以免出错后可以恢复。
```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

第二步：下载新的CentOS-Base.repo 到/etc/yum.repos.d/
```
CentOS 5
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
CentOS 6
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
CentOS 7
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```
第三步：运行yum makecache生成缓存
```
yum clean all
yum makecache
```

增加dockerHub加速器
OPTIONS='--selinux-enabled --log-driver=journald --registry-mirror=https://yy71r5tm.mirror.aliyuncs.com'
