#add_ceph

安装说明
========

- 此项目主要是实现计算节点ceph的扩容

- 一、主要实现添加计算节点的本地源(需要通过已有的openstack缓存做本地源)，然后安装一些ceph基础包.
- 二、安装计算节点ceph的osd

- 三、更改crush map扩容ceph



第一步: 更新本地源和安装ceph基础包
--------------

* 修改hosts和group_vars/all的yum本地源ip
* 执行脚本: ansible-playbook -i hosts add_ceph_pre.yml

第二步: 在计算节点安装ceph的osd
--------------

* 修改group_vars/all的add_ceph_osd的变量，和原来的openstack集群的ceph配置一致
* 安全起见先执行: ansible-playbook -i hosts add_ceph_osd.yml
* 注意一定不要用-f并发，否则osd的顺序是乱掉的,还需要验证


第三步: 更改crush map
--------------

* 使用ceph osd tree，看看新添加的osd状态是否都是up
* 使用命令按照每台服务器加入到各自的domain
* 使用ceph osd tree验证新机器是否加入domain
* 等几个小时ceph同步数据，这几个小时不要创建虚机
