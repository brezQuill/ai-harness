---
name: Open vSwitch (OVS) OpenFlow 命令参考手册
---
### 流表增删改查
* 查询流表规则
```bash
# 命令示例
ovs-ofctl dump-flows br0 table=230
# 结果示例
 cookie=0x60007, duration=169372.816s, table=230, n_packets=130925, n_bytes=35544877,priority=50095,reg1=0xd345e3,reg2=0x5254,metadata=0x5 actions=load:0x64000425->NXM_NX_REG4[],load:0x64->NXM_NX_REG3[],resubmit(,22)
```
* 添加流表规则
```bash
# 命令格式
ovs-ofctl add-flow <bridge> "<match_fields>,actions=<actions>"
# 命令示例
ovs-ofctl add-flow br0 "priority=100,in_port=1,actions=output:2"
# 结果示例
成功添加规则
```
* 删除流表规则
```bash
# 命令格式
ovs-ofctl del-flows <bridge> [match_expression]
# 命令示例
ovs-ofctl del-flows br0 "in_port=1"
```
* 修改流表规则
```bash
# 命令格式
ovs-ofctl mod-flows <bridge> "<match_fields>,actions=<actions>"
# 命令示例
ovs-ofctl mod-flows br0 "in_port=1,actions=drop"
```
### 常用匹配字段
* 基础匹配字段
```bash
in_port=<port>              # 入口端口
dl_src=<mac>                # 源MAC地址
dl_dst=<mac>                # 目的MAC地址
dl_type=0x0800              # IPv4协议
dl_type=0x86dd              # IPv6协议
```
* IP层字段匹配
```bash
ip,nw_src=<ip>[/<mask>]    # 源IP地址
ip,nw_dst=<ip>[/<mask>]    # 目的IP地址
nw_proto=6                 # TCP协议
nw_proto=17                # UDP协议
nw_proto=1                 # ICMP协议
```
* 传输层字段匹配
```bash
tcp_src=<port>             # TCP源端口
tcp_dst=<port>             # TCP目的端口
udp_src=<port>             # UDP源端口
udp_dst=<port>             # UDP目的端口
```
* VLAN字段匹配
```bash
dl_vlan=<vlan_id>          # VLAN ID
dl_vlan_pcp=<priority>     # VLAN优先级
```
### 常用动作参考
* 输出动作
```bash
output:<port>              # 输出到指定端口
output:NORMAL              # 正常L2/L3转发
output:ALL                 # 洪泛到所有端口
output:CONTROLLER          # 发送到控制器
output:IN_PORT             # 从入口端口返回
output:FLOOD               # 洪泛
output:LOCAL               # 输出到本地协议栈
```
* 修改动作
```bash
mod_vlan_vid:<vlan_id>     # 设置VLAN ID
strip_vlan                 # 剥离VLAN标签
mod_dl_src:<mac>           # 修改源MAC
mod_dl_dst:<mac>           # 修改目的MAC
mod_nw_src:<ip>            # 修改源IP
mod_nw_dst:<ip>            # 修改目的IP
mod_tp_src:<port>          # 修改传输层源端口
mod_tp_dst:<port>          # 修改传输层目的端口
```
* 控制动作
```bash
drop                       # 丢弃数据包
resubmit(<table>)          # 重新提交到指定表
goto_table:<table>         # 跳转到指定表
meter:<meter_id>           # 应用计量表
group:<group_id>           # 应用组表
```


