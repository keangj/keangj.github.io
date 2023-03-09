

## 

打开终端使用 `ssh` 登录群晖账户

``` sh
ssh -p 22 <用户名>@<群晖 IP 地址>
// 然后输入你的密码
cd /dev/dri
ls 	// 如果出现 card0 renderD128 证明支持硬件解码
sudo -i 	// 获得 root 权限
```

用命令行创建 `docker` 容器

``` sh
docker run --name=<contianerName>
```
