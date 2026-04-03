---
name: ovs ct-limit 命令参考手册
---
* 设置CT会话总数
```bash
# 命令示例
ovs-appctl dpctl/ct-set-maxconns 10000000
# 结果示例
setting maxconns successful
```
* 获取dp会话总数
```bash
# 命令示例
ovs-appctl dpctl/ct-get-maxconns
# 结果示例
10000000
```
* 设置某zone的会话数限制值
```bash
# 命令格式
ovs-appctl dpctl/ct-set-limits [dp] [default=L] [zone=N,limit=L]...
# 命令示例
ovs-appctl dpctl/ct-set-limits zone=100,limit=4000000
```
* 获取某zone及default或所有的会话数限制，以及zone内会话数
```bash
# 命令格式
ovs-appctl dpctl/ct-get-limits [dp] [zone=N1[,N2]...]
# 命令示例
ovs-appctl dpctl/ct-get-limits
# 结果示例
default limit=5000000
zone=100,limit=4000000,count=0
```

* 获取某zone及default或所有的会话数限制，以及相应的会话数
```bash
# 命令格式
ovs-appctl dpctl/ct-get-limits [dp] [zone=N1[,N2]...]
# 命令示例
ovs-appctl dpctl/ct-get-limits
# 结果示例
default limit=5000000
zone=100,limit=4000000,count=0
```

* 删除某zone的会话数限制
```bash
# 命令格式
ovs-appctl dpctl/ct-del-limits [dp] zone=N1[,N2]...
# 命令示例
ovs-appctl dpctl/ct-del-limits zone=100
```