##  docker

### 1.配置镜像加速器

```
在/etc/docker/daemon.json写入：
{"registry-mirrors":["https://8kb0360u.mirror.aliyuncs.com/"]}

之后重启服务
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

