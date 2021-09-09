## HW4
新建一个用户Admin
1. 密码100天后过期，提前7天提醒，过期10天后不修改密码将关闭账号
2. 定义一个SHELL，可以选择bash，也可以选择zsh，有HOME
3. 登录打印自定义欢迎信息：Welcome Admin（此处取变量）！Today is yyyy-mm-dd 并且打印最近一次登录的详细信息和近期登录总次数。
4. 可直接执行my_ls，my_ls是你自己写的ls程序，如果不会写，你可以写一个简单的HelloWorld代替。可以直接执行的意思是，在任何地方直接执行my_ls可以运行。
5. 可执行sudo命令

## 实现
* 为了实现可以选择bash，也可以选择zsh，有HOME，需要用到`useradd -m` 和`-s`
* 为了执行sudo命令，需要在用户名后加 `-G/-g`
```
useradd -m -s /bin/zsh Admin -g sudo
```
* 密码相关配置需要用到passwd命令，其中
* 100天后过期过期用 `-x 100`
* 过期10天后不修改密码将关闭账号`-i 10`
* 提前7天提醒用 `-w 7`
```
passwd -w 7 -x 100 -i 10 Admin
```


* 登录打印自定义欢迎信息需要在`~/.zshrc`里增加登录信息，代码如下
```
echo "Welcome $(whoami) ! Today is $(date +%Y-%m-%d)"
cnt=`last | cut -d " " -f 1 | grep "$(whoami)" | wc -l`
echo "Recently you have logged in $cnt times."
echo "last logged in information:"
w
```
效果图
![image-20210909154733902](C:\Users\weiran.shao\AppData\Roaming\Typora\typora-user-images\image-20210909154733902.png)

* 为了可直接执行my_ls，在`./bashrc`中增加`export PATH="/home/Admin/scripts:$PATH"`
之后运行`source ~/.bashrc`就把脚本地址增加到全局变量中了，之后运行`chmod +777 my_ls`修改脚本mode，之后运行脚本，效果如下
![image-20210909155436923](C:\Users\weiran.shao\AppData\Roaming\Typora\typora-user-images\image-20210909155436923.png)

