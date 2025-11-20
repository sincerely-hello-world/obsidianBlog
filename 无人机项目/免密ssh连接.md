1. ssh-keygen产生 公钥和私钥

```pwsh
## C:\Users\since\.ssh
ssh-keygen -t rsa -b 4096 -f C:\Users\since\.ssh\orangepi_local
```

2. 公钥pub放server(远程主机)上，私钥放本机上，路径在`C:\Users\since\.ssh\orangepi_local`
3. 把.pub后缀的公钥传输到server的`~/.ssh/`文件夹中 （可以用scp命令）
4. - 执行`cat ~/orange_local.pub >> ~/.ssh/authorized_keys`命令，将公钥文件信息写入`authorized_keys`文件（`cat`命令使用`>`符号时，若文件不存在会自动创建。`>`代表覆盖，`>>`代表追加）
5. - 执行`service sshd restart`或者`sudo service sshd restart`重启`sshd`服务（如果服务器版本过高可能会要求使用`systemctl restart sshd`）
6. 同时，由于ssh不希望`home`目录以及`~/.ssh`目录对组有写权限，所以需要对目录进行权限更改。同时，ssh对于authorized_keys也有权限需求。

  

作者：维生素C  
链接：https://juejin.cn/post/7023042621295558692  
来源：稀土掘金  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。