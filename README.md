# 一键 V2ray websocket + TLS

一键就完事了，扫描二维码 或者 复制 vmess链接 无需关心复杂的V2ray 配置，websocket + tls 更安全，伪装更好。

* 自动生成 UUID （调用系统UUID库）
* 默认使用 caddy 自动获取证书
* 自动生成 安卓 v2rayNG vmess链接
* 自动生成 iOS shadowrocket vmess链接
* 自动生成 iOS 二维码

## 使用方法

* 提前安装好docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh  && \
bash get-docker.sh
```

* 解析好域名 确认 你的域名正确解析到了你安装的这台服务器
* 会占用 443 和 80 端口请提前确认没有跑其他的业务 （ lsof -i:80 和 lsof -i:443 能查看）
* 请将下面命令中的 YOURDOMAIN.COM（域名）替换成自己的域名（此IP解析的域名）！！！
* 可将下面命令中的 one 替换成任意的路径（不要用v2ray这种可能被墙探测的路径）

```bash
sudo docker run -d --rm --name v2ray -p 443:443 -p 80:80 -v $HOME/.caddy:/root/.caddy  congxy/v2ray_ws:0.2 YOURDOMAIN.COM one && sleep 3s && sudo docker logs v2ray
```

* 如果你想指定固定 uuid 的话， 0890b53a-e3d4-4726-bd2b-52574e8588c4 这个 uuid 改为你自己的，<https://www.uuidgenerator.net/> 这个网站可以生成随机 uuid。

```bash
sudo docker run -d --rm --name v2ray -p 443:443 -p 80:80 -v $HOME/.caddy:/root/.caddy  congxy/v2ray_ws:0.2 YOURDOMAIN.COM one 0890b53a-e3d4-4726-bd2b-52574e8588c4 && sleep 3s && sudo docker logs v2ray
```

* 命令执行完会显示链接信息，如果想查看链接信息，执行下面命令即可

```bash
sudo docker logs v2ray
```

* 想停止这个 docker 和服务

```bash
sudo docker stop v2ray
```

## ipv6 支持

* 由于docker默认网络只支持ipv4，所以即使VPS有ipv6地址也无法通过v2rayDocker访问ipv6网站，但是我们可以使用 <https://github.com/robbertkl/docker-ipv6nat> 为 Docker 提供 IPv6 NAT 功能。
* 首先，编辑 /etc/docker/daemon.json 文件，加入以下内容：

```json
{
    "ipv6": true,
    "fixed-cidr-v6": "fc00::/7"
}
```

* 然后，启动镜像

```bash
docker run -d --restart=always -v /var/run/docker.sock:/var/run/docker.sock:ro --privileged --net=host robbertkl/ipv6nat
```

* 启动之后, -p 就会同时开放容器内的ipv4和ipv6端口，重启v2ray即可。

感谢pengchujin
