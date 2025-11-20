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
orange_local.pub
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCsoZceN0uL16pPGhsinCco0VgehFhJzxa9KdSZCMPvhDi0z+QJpsfNZTlPPyhpAkZEY//udieukBUseDXUF1sPVv4G3qfB5dwrz3MP83irmfys4ljyD4c/5RvnIWey4Dg+7DkI7IRNTnuJLuMcj1kbtJObq5Tk/1zfL9i0Kmr0mbJhHYuVtZeIGOqGsFiD1cmByeVebeo9I49v+KAA3H6KvHwNlBYVbZ+Dr8Rg5uMoUEFO5CHYpExIE4K7koDaZX9aLU/JO2M9Z2t+cpCYVHI9i5hs3POhHObsmASdJiYGCC9RiBAvMVKlnOGLQ8djcfE+a0WEGk/uSNun/xYqBtFVnlfJsAC72TWpnE4waGC6iYtyn99zjMsnWv5svQaXP0jJkD+lAHJe4e7p6fcOpzcRsKsnQC1rmC5XkrXmBMX53XkGejpD8BxOgQTW6/wd/W8ZRhVs2l/f4tRJJ+Y/0oSWW8hDsogA9QiAz3F3lr12lZkcByZ7whD8ybWzFiGrAwHbEcwE/+7r9EhoJIyas8NFvf6ps3qRa3nMnrZKjina3k33wHuFPeqcqiUXWfAcluOHqu/3R3zof+23a57NCjaXLPeO1XIcX6ACC6+yT4iUAMquKNEiiGpG+fkE8yL6Qz1oItGQtyXjBxJn08BeZ+zoh+7dUn+lqySpfWGtZvTzFQ== since@dawn
```

orange