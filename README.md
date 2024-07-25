# 关于kvm的剧本
目前安装kvm环境和网卡初始化肯能只适用cento7，其他环境未做测试
痛点：
  copy镜像的时候会比较慢，如果同机柜或者内网的话还能接受
  需要提前制作一份centos7.9.2009-15G.qcow2镜像文件（镜像名可自定义）放到copy-image/files/下

# 使用方法

## 关于变量

```
  vars:
    - image_name: centos7.9.2009-15G   # 这个在copy-image时需要先上传到相关role的file中，是kvm克隆的源文件
    - instance_name: test-10  # 实例的主机名
    - instance_passwd: passwd # 实例的root密码
    - cpu: 3                  # 实例cpu核心
    - memory: 3G              # 实例的内存
    - instance_ip: ip         # 实例的ip 
    - instance_gw: 192.168.10.2 # 实例的网关
```



## 安装相关程序包
```
ansible-playbook -t init_kvm -e "hosts=kvm" kvm.yaml
```
## 创建桥接网卡
目前应该不支持双网卡的情况（待补充）
```
ansible-playbook -t br0 -e "hosts=kvm" kvm.yaml  # 这个使用的是virt命令创建的网卡

ansible-playbook -t br0_m -e "hosts=kvm" kvm.yaml # 这个是通过查找到默认网卡对原网卡配置进行注释，手工生成桥接网卡的配置，在重启网络，这部分暂时注释，使用上面的方式比较快 
```

## 将克隆虚拟机的所需镜像与xml文件复制到目标主机

```
ansible-playbook -t copy-image -e "hosts=kvm" kvm.yaml
```

## 开始克隆虚拟机

```
ansible-playbook -t create-kvm -e "hosts=kvm" kvm.yaml

```
## 对虚拟机进行ip，主机名初始化
```
ansible-playbook -t init-vm -e "hosts=kvm" kvm.yaml
```
## 清理复制过去的镜像，xml文件和初始化脚本
```
ansible-playbook -t clean -e "hosts=kvm" kvm.yaml
```

## 可通过直接-e 指定变量安装
```
ansible-playbook  -e "hosts=kvm image_name=centos7.9.2009 instance_name=test-50 instance_passwd=redhat cpu=3 memory=3G instance_ip=192.168.10.50 instance_gw=192.168.10.2" kvm.yaml
```

