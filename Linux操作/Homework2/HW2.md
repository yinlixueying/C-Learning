# HW2
* 要求：在每次通过远程连接进⼊系统时，告知⽤户信息
因为要求是在进入系统时打印消息，所以所有的打印都放在 /etc/zsh/zlogin 文件里
## 整体输出
* ![image-20210908111240192](C:\Users\weiran.shao\AppData\Roaming\Typora\typora-user-images\image-20210908111240192.png)
## Task 1
* 最近⼀段时间，当前⽤户，也就是你，登录了多少次 
	操作流程：
	1. 运行last命令
	2. 以“ ”为分隔符，cut出第一列
	3. 用whoami命令查出当前用户名称
	4. 用grep命令在cut出的一列中找出所有当前用户的行
	5. 利用wc命令数出有多少行，也就是登录的次数
	``cnt=`last | cut -d " " -f 1 | grep  "$(whoami)" | wc -l` ``
	
	输出：	
	![image-20210908113352320](C:\Users\weiran.shao\AppData\Roaming\Typora\typora-user-images\image-20210908113352320.png)

## Task 2
* 上⼀次登录系统，停留了多少时间
	这个任务要记录login和logout中间有多长时间，因此/etc/zsh/zlogin和/etc/zsh/zlogout文件都需要修改
	整体流程：
	1. 在login文件中，打印上一次停留的时间 `b=$(cat out >&1)`
	2. 运行 `date +%s > out` 将当前时间戳写入out文件
	3. 在logout文件中记录当前时间戳`a=\`date +%s\``
	4. 将out文件中的login时间戳提取出来
	5. 停留时间 = logout时间戳 - login时间戳
	6. 将停留的时间写入out文件
	login文件代码：
```
b=$(cat out >&1)
echo "-----------1. Last time you have been there for $b seconds.--------"
date +%s > out
```
logout文件代码：
```
a=`date +%s`
b=$(cat out >&1)
let c=a-b
echo "$c" > out
```
## Task 3
* 给⽤户推荐⼀句名⼈名⾔，唐诗三百⾸之类的
此命令利用了linux里的fortune命令
![image-20210908115024446](C:\Users\weiran.shao\AppData\Roaming\Typora\typora-user-images\image-20210908115024446.png)

## Task 4
* 告知当地当⽇天⽓预报
此命令利用了`curl wttr.in`命令，直接按ip地址给当地的天气预报
效果图：
![image-20210908115114330](C:\Users\weiran.shao\AppData\Roaming\Typora\typora-user-images\image-20210908115114330.png)
## Task 5
* ⼀个温馨的问候
问候语写在/etc/motd 中。写的词是“Welcome”
![image-20210908115049234](C:\Users\weiran.shao\AppData\Roaming\Typora\typora-user-images\image-20210908115049234.png)

## 附录（代码）
/etc/zsh/zlogin 文件
```
b=$(cat out >&1)
echo "-----------1. Last time you have been there for $b seconds.--------"
date +%s > out
cnt=`last | cut -d " " -f 1 | grep  "$(whoami)" | wc -l`
echo "-----------2. Recently you have login $cnt times.------------------"
echo "-----------3. Daily fortune:---------------------------------------"
fortune
echo "-----------4. Today's weather:-------------------------------------"
curl wttr.in
```
/etc/zsh/zlogout 文件
```
a=`date +%s`
b=$(cat out >&1)
let c=a-b
echo "$c" > out
```
/etc/motd 文件
```
oooooo   oooooo     oooo           oooo
 `888.    `888.     .8'            `888
  `888.   .8888.   .8'    .ooooo.   888   .ooooo.   .ooooo.  ooo. .oo.  .oo.
   `888  .8'`888. .8'    d88' `88b  888  d88' `"Y8 d88' `88b `888P"Y88bP"Y88b
    `888.8'  `888.8'     888ooo888  888  888       888   888  888   888   888
     `888'    `888'      888    .o  888  888   .o8 888   888  888   888   888
      `8'      `8'       `Y8bod8P' o888o `Y8bod8P' `Y8bod8P' o888o o888o o888o

```