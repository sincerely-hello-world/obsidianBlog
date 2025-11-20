1. ssh-keygen产生 公钥和私钥

```pwsh
## C:\Users\since\.ssh
ssh-keygen -t rsa -b 4096 -f C:\Users\since\.ssh\orangepi_local
```


2. 公钥pub放server(远程主机)上，私钥放本机上，路径在`C:\Users\since\.ssh\`
3. 把.pub后缀的公钥传输到server的`~/.ssh`文件夹中 （可以用scp命令）
```

```

4. - 执行`cat ~/id_rsa.pub > ./.ssh/authorized_keys`命令，将公钥文件信息写入`authorized_keys`文件（`cat`命令使用`>`符号时，若文件不存在会自动创建。`>`代表覆盖，`>>`代表追加）