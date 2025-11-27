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

```
chmod 700 ./.ssh 
chmod 600 ./.ssh/authorized_keys
```
  

作者：维生素C  
链接：https://juejin.cn/post/7023042621295558692  
来源：稀土掘金  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

---
## `orange_local.pub` 公钥 放“开发板/服务器” `~/.ssh/orange_local.pub`
+ 放目标服务器 ~/.ssh/orange_local.pub
+ cat ~/orange_local.pub >> ~/.ssh/authorized_keys
+ 执行`service sshd restart`或者`sudo service sshd restart`或者`systemctl restart sshd`重启`sshd`服务
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCsoZceN0uL16pPGhsinCco0VgehFhJzxa9KdSZCMPvhDi0z+QJpsfNZTlPPyhpAkZEY//udieukBUseDXUF1sPVv4G3qfB5dwrz3MP83irmfys4ljyD4c/5RvnIWey4Dg+7DkI7IRNTnuJLuMcj1kbtJObq5Tk/1zfL9i0Kmr0mbJhHYuVtZeIGOqGsFiD1cmByeVebeo9I49v+KAA3H6KvHwNlBYVbZ+Dr8Rg5uMoUEFO5CHYpExIE4K7koDaZX9aLU/JO2M9Z2t+cpCYVHI9i5hs3POhHObsmASdJiYGCC9RiBAvMVKlnOGLQ8djcfE+a0WEGk/uSNun/xYqBtFVnlfJsAC72TWpnE4waGC6iYtyn99zjMsnWv5svQaXP0jJkD+lAHJe4e7p6fcOpzcRsKsnQC1rmC5XkrXmBMX53XkGejpD8BxOgQTW6/wd/W8ZRhVs2l/f4tRJJ+Y/0oSWW8hDsogA9QiAz3F3lr12lZkcByZ7whD8ybWzFiGrAwHbEcwE/+7r9EhoJIyas8NFvf6ps3qRa3nMnrZKjina3k33wHuFPeqcqiUXWfAcluOHqu/3R3zof+23a57NCjaXLPeO1XIcX6ACC6+yT4iUAMquKNEiiGpG+fkE8yL6Qz1oItGQtyXjBxJn08BeZ+zoh+7dUn+lqySpfWGtZvTzFQ== since@dawn
```


## `orange_local` 私钥 放本地 `C:\Users\since\.ssh\orangepi_local`
```
-----BEGIN OPENSSH PRIVATE KEY-----
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
ZH9Y2b5OLOkAAAAKc2luY2VAZGF3bgE=
-----END OPENSSH PRIVATE KEY-----
```
 + vsc的ssh config
```
Host orange_local
  User root
  HostName 192.168.31.116
  Port 22
  IdentityFile C:\Users\since\.ssh\orangepi_local
```