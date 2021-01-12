加速度 目前只有一个z轴方向被保留

moving mean 1分钟 是不是有点长



CNIX: 邀请码：`https://邀请01.很有精神.com/auth/register?code=eI82`

`tmux`

---

ssh root@59.78.28.3 -p 24000

59.78.28.3

---

### Influxdb

`service influxdb start` to start influxdb daemon

`influx -precision rfc3339` to start influxdb CLI

---

### Grafana

[With Grafana | Grafana Labs](https://grafana.com/docs/grafana/latest/getting-started/getting-started/)

---

2020/12/29:

* 时间时区好像有问题
* 

---

2021/1/13

influxdb 的python 操作较为奇怪

* 注意influxdb有tag和field两种属性，tag可能是用红黑树，可以快速索引，field一定要有
* measurement就相当于是table
* TODO: 没找到合适的query方式，不知是否可以尝试influxdb-alchemy

目前用了两个表usertable和data

* usertable和data都要存储public_id
  * public_id记录了user的唯一标识符uuid4，区别于swift提供的uuid

TODO：

* error response需要明确
* 鲁棒性
* curl postman 对于虚拟机的支持
  * VMWare 3种模式，尤其是NAT模式，不知道要怎么解决连接问题
* 学习HTTPS理论