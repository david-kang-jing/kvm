# kvm
## 关于kvm的测试
目前只适用于centos7
### 创建桥接网卡，为创建虚机做准备
```
vim crate_br0.yaml  # 需先指定网卡名称，和要修改后的ip和掩码相关
ansible -e "hosts=$IP" crate_br0.yaml
```
