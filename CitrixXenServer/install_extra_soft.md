# XenServer. Install extra software

## Option 1: use a repo
```shell
sed -i 's/\$releasever/7/g' /etc/yum.repos.d/CentOS-Base.repo
yum install tmux --enablerepo=base
```

## Option 2: directly from site

**ncdu**
```shell
rpm -i https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/n/ncdu-1.18-1.el7.x86_64.rpm
```
