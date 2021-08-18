# 本地生成密钥

### mac方法

1. 生成密钥命令
```
ssh-keygen -t rsa -C '1289206613@qq.com'
```

- 一路回车。如下：
----------------------

![](../images/secretKey.jpg)

---------------------

- 密钥地址`/Users/liangao/.ssh/id_rsa`
- 打开密钥，终端输入`open /Users/liangao/.ssh`
- 查看`id_rsa`文件

2. 打开隐藏文件
```
// 第一步
defaults write com.apple.finder AppleShowAllFiles TRUE
// 第二步
killall Finder
```

3. 访问root用户

- 如果生成密钥的地址在`/var/root/.ssh/`文件内

[苹果管网解决办法](https://support.apple.com/zh-cn/HT204012)
