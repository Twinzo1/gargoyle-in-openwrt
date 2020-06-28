# gargoyle-in-openwrt
### Transplant gargoyle packages to openwrt

### 移植石像鬼插件到openwrt

#### Useage:

1. 将整个项目克隆到openwrt源码package目录中，全部放入gargoyle目录中

2. 在执行一次make menuconfig选择好target后，执行：
```
package/gargoyle/gargoyle-build.sh
```
3. 再次执行
```
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
```

#### Update:

该项目完全可以自己更新，所需要修改的只有少量脚本。
1. 除了 

* ./netfilter-match-modules/integrate_net_modules.sh
* ./patches/qos-gargoyle.patch
* ./patches/qosmon.patch
* ./gargoyle-build.sh

其它文件可以从 https://github.com/ericpaulbishop/gargoyle 中直接替换

2. 更新 ./netfilter-match-modules/integrate_net_modules.sh

主要添加了判断补丁是否已经存在，防止重复添加：
```
### 判断是否存在该模块
ifExist1=`cat "../$kernel_netfilter_mk" | grep "ipt-${lower_name}" 2>/dev/null 2>&1`
if [ -n "$ifExist1" ]; then
  echo "Already exists"
else
  ...
fi

#判断是否存在该模块
ifExist2=`cat "../$iptables_makefile" | grep "iptables-mod-${lower_name}" 2>/dev/null 2>&1`
if [ -n "$ifExist2" ]; then
  echo "Makefile Already exists"
else
  ...
fi

#判断是否存在该模块
ifExist3=`cat "../include/netfilter.mk" | grep "${upper_name}" 2>/dev/null 2>&1`
if [ -n "$ifExist3" ]; then
  echo "include/netfilter.mk Already exists ${upper_name}"
else
  ...
fi
```

3. qos补丁
```
./patches/qos-gargoyle.patch
./patches/qosmon.patch
```
是为了使它可以正常编译而添加的，若去掉可正常编译，则可去。

#### Thanks:

* https://github.com/ericpaulbishop/gargoyle

* https://github.com/project-openwrt/openwrt/commit/47040d246daceff23739e44f4cbb38ebbf3bb593
