# k8s-jenkins
1、搭建NFS 服务端(192.168.100.12)

我们可以在k8s集群外的一台主机上搭建一个nfs server。

yum install rpcbind  nfs-utils -y

mkdir /nfs_data

chmod +w /nfs_data

chmod +x /nfs_data

vi /etc/exports,内容如下：
/nfs_data  192.168.100.0/24(rw,no_root_squash)

启动nfs服务

service nfs start #启动nfs服务
service rpcbind start   #启动rpc服务

showmount  -e 192.168.100.12 #查看共享盘 ok

 

2、安装nfs-client（存储类）
 
git clone https://github.com/helm/charts.git
cd charts/
#实例化一个Release ，指定Release名称为mynfs-client，并设置为默认的存储类
helm install stable/nfs-client-provisioner  \
--set nfs.server=192.168.11.24 --set nfs.path=/nfs_data \
--set storageClass.defaultClass=true \
--name nfs-storage
