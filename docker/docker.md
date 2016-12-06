安装docker：
yum install -y docker

编辑/etc/sysconfig/docker文件，配置docker加速器和仓库：
```
# Modify these options if you want to change the way the docker daemon runs
OPTIONS='--selinux=enabled --iptables=false --log-driver=journald --registry-mirror=https://yy71r5tm.mirror.aliyuncs.com'
DOCKER_CERT_PATH=/etc/docker

# If you want to add your own registry to be used for docker search and docker
# pull use the ADD_REGISTRY option to list a set of registries, each prepended
# with --add-registry flag. The first registry added will be the first registry
# searched.
#ADD_REGISTRY='--add-registry registry.access.redhat.com'

# If you want to block registries from being used, uncomment the BLOCK_REGISTRY
# option and give it a set of registries, each prepended with --block-registry
# flag. For example adding docker.io will stop users from downloading images
# from docker.io
# BLOCK_REGISTRY='--block-registry'

# If you have a registry secured with https but do not have proper certs
# distributed, you can tell docker to not look for full authorization by
# adding the registry to the INSECURE_REGISTRY line and uncommenting it.
INSECURE_REGISTRY='--insecure-registry registry.cn-hangzhou.aliyuncs.com'
```
registry-mirror 加速器配置
INSECURE_REGISTRY 仓库地址配置

启动docker服务：
systemctl enable docker.service && systemctl start docker.service
