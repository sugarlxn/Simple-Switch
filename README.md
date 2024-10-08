# Simple-Switch

```
         _                 __                         _ __       __  
   _____(_)___ ___  ____  / /__        ______      __(_) /______/ /_ 
  / ___/ / __ `__ \/ __ \/ / _ \______/ ___/ | /| / / / __/ ___/ __ \
 (__  ) / / / / / / /_/ / /  __/_____(__  )| |/ |/ / / /_/ /__/ / / /
/____/_/_/ /_/ /_/ .___/_/\___/     /____/ |__/|__/_/\__/\___/_/ /_/ 
                /_/                                                  

```

**[中文](./README.md) / [English](./README_en.md)**

这个是一个基于P4语言实现的简单2层交换机，使用9180x32 p4交换机 sde9.2.0

实现的功能：

- 交换机2层转发
- mac地址自学习、老化，即支持端口热插拔
- arp请求广播


## 编译

```
./<path to your sde9.2.0>/p4_build.sh ./<path to this project>/my_simple_l2.p4
```

## 如何使用

```
# 运行p4程序
./run_switch.sh -p my_simple_l2
# 使用bfshell 运行setup脚本
./run_bfshell.sh -b ./my_simple_l2_setup.py -i 
```

- 如果出现以下错误信息：
```
error opening /dev/bf0
ERROR: Device mmap failed for dev_id 0
please load driver with bf_kdrv_mod_load script. Exiting...
```
执行以下命令：
```
$SDE/install/bin/bf_kdrv_mod_load $SDE_INSTALL
# SDE=/root/bf-sde-9.2.0
# SDE_INSTALL=/root/bf-sde-9.2.0/install
```

- bfshell简单使用, 配置端口，使能端口，查看端口状态
```
bfshell> ucli
pm
# 添加端口 port-add <port_nanme> <speed> <mode> 
port-add -/- 10G none
port-add 10/- 10G none
# 使能端口
port-enb -/-
port-enb 10/-
# 查看已使能端口信息
show
# 查看所有端口状态
show -a
```
- bfshell 查看流表
```
bfshell> bfrt_python
bfrt_python> bfrt
bfrt> my_simple_l2
bfrt.port> pipe
bfrt.port.pipe> ingress
```

## 贡献

PRs accepted.

## 许可证

MIT © Richard McRichface
