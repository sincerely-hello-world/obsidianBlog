
```text
nano ~/.ssh/config  # 无此文件则新建
```

```
# # 方案A：HTTP/SOCKS5代理（需替换IP和端口）
# Host github.com
#   ProxyCommand nc -X 5 -x 代理IP:端口 %h %p  # SOCKS5
#   # ProxyCommand nc -X connect -x 代理IP:端口 %h %p  # HTTPS

# 方案B：直接使用443端口（通用性强）
Host github.com
  Hostname ssh.github.com
  Port 443
```