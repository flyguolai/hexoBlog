## 自用的命令

### docker clash

```bash
    docker run -d --name clash --log-opt max-size=1m --network host -v $(pwd)/clash-config:/root/.config/clash -p 7890:7890 dreamacro/clash
```


### docker 代理

```bash
# /etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=192.168.0.xxx:7890"
Environment="HTTPS_PROXY=192.168.0.xxx:7890"
Environment="NO_PROXY=localhost,127.0.0.1"
```