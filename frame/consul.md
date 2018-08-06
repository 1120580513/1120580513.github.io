# Consul(服务发现)

---

## Init
> https://www.consul.io/downloads.html
> 加入环境变量，输入consul，打印内容表示正常

## CMD
```cmd
192.168.80.100>consul agent -server -ui -bootstrap-expect=3 -data-dir=/tmp/consul -node=consul-1 -client=0.0.0.0 -bind=192.168.80.100 -datacenter=dc1

192.168.80.101>consul agent -server -ui -bootstrap-expect=3 -data-dir=/tmp/consul -node=consul-2 -client=0.0.0.0 -bind=192.168.80.101 -datacenter=dc1 -join 192.168.80.100

192.168.80.102>consul agent -server -ui -bootstrap-expect=3 -data-dir=/tmp/consul -node=consul-3 -client=0.0.0.0 -bind=192.168.80.102 -datacenter=dc1 -join 192.168.80.100
```