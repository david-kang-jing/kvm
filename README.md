# 关于kvm的剧本
目前只适用于centos7,相关判断条件是centos7

# 使用方法

## 关于变量

```
  vars:
    - image_name: centos7.9.2009-15G   # 这个在copy-image时需要先上传到相关role的file中，是kvm克隆的源文件
    - instance_name: test-10  # 克隆的目标主机的名称
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

