## redis安装和部署

### 1.redis的安装

```
docker pull  redis:3.2
docker run -p 6379:6379 -d --name redis redis:3.2 redis-server --appendonly yes
docker exec -it redis redis-cli
```


