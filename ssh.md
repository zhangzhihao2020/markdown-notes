# 利用ssh协议访问github
1.生成公钥
```
cd ~/.ssh
ssh-keygen -t rsa -C "你的github邮箱地址"
sudo gedit /home/你的电脑用户名/.ssh/id_rsa.pub
```
复制id_rsa.pub内容
2.设置GitHub
打开GitHub账号，点击头像，找到`settings`
进入`settings`点击`SSH and GPG keys`
进入`SSH and GPG keys`点击`New SSH key`
 	`Title`任意
 	`Key`填入复制id_rsa.pub内容
 	然后点击`Add SSH key`
3.打开终端，输入
```
ssh -T git@github.com
```
显示` Hi zhangzhihao2020! You've successfully authenticated`,操作正确
4.配置git

```
sudo gedit ~/.gitconfig
```
输入：
```
[core]
    gitproxy = git-proxy
[socks]
    proxy = localhost:1089
[user]
    name = 你的github用户名
    email = 你的github注册邮箱
```
 proxy = localhost:1089中1089和Qv2ray中SOCKS一致

