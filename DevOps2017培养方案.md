## DevOps2017培养方案
### DevOps = Culture + Tools
- 之后的任务，每周都需要提交代码，如果周期为两周的任务，也需要每周提交一次。学会 Git 基本使用后，代码统一传到工作室 Git 上。
- 提问的技巧：
  - 不要问 `有没有人用过xxx/有没有人在` 这种问题，有问题直接问就行，大家有时间能够帮忙解答都会帮忙的。
  - 提问尽量避免发自己的代码压缩包，自己把跟出现问题相关部分的代码做成在线的demo，例如: [codepen](https://codepen.io/)，[jsbin](http://jsbin.com/?html,css,js,output)，把demo的链接贴出来就好了。
  - 不要因为自我感觉问题太低级而害怕提问，但是在提问前一定要有自己的思考过程。
  - 学习过程中有困惑的地方可以多与其他同学交流，切忌闭门造车。
- 以下内容同样为必学内容，并且是长期学习的内容，时间安排根据自己的学习进度来定，越早学越好。（无先后顺序）
   - Git（要求：能够理解分支概念和回滚）
   - Markdown（要求：熟悉常用的语法）
   - 不同Linux版本的软件管理
   - shell脚本的基本应用（必备）
   - python开发、perl
   - 容器平台（docker）
   - vim编辑器的使用
   - 网络基础的建立
   - Nginx与Openresty
   - 正则表达式
   - 文件格式化处理
   - Linux文件、目录、文件系统管理
   - 熟悉基础服务（LNMP、LAMP、DNS）
   - 底层：Linux C、内核
   - 安全：防火墙配置（iptables、firewalld）
 
- 论坛或者社区
    - Github
    - V2EX
    - 知乎
    - 掘金
    - 简书
-  相关文章
    - [运维如何通过学习python学会编程](https://github.com/pythonpeixun/article/blob/master/python/how_to_learn_python.md)
    - [知道创宇研发技能表](http://blog.knownsec.com/Knownsec_RD_Checklist/v3.0.html)
    - [简明vim练级攻略](https://coolshell.cn/articles/5426.html)
    - [Linux系统结构详解](http://blog.csdn.net/hguisu/article/details/6122513)
    - [每天一条Linux命令](http://www.cnblogs.com/peida/archive/2012/12/05/2803591.html)
    - [Linux命令大全](http://man.linuxde.net)
    - [ChinaUnix](http://bbs.chinaunix.net/)
    - [Linux 平台上的软件包管理](https://www.ibm.com/developerworks/cn/linux/l-cn-rpmdpkg/index.html)
    - [Linux中国](https://linux.cn/)
- 空闲时间可以自己去搭建一个属于自己的博客或是买个VPS翻翻墙
- 每个人都有适合自己的学习方式，希望这个培养方案能够帮助你找到最适合自己的学习方式并快速成长起来。

###Linux基本命令操作
- 时间：两周
- 技能点：熟悉基本的命令行操作
- 考核内容：
	- 将使用命令行时需要注意的地方体现到周报及博客上
	- 常见问题排解：如忘记root密码

###vim编辑器的使用
- 时间：一周
- 技能点：较熟练的使用vim、vim环境设置
- 考核内容：
	- 打开vim后依次输入：`it<Esc>$ae<Esc>ih<Esc>ea s<Esc>ii<Esc>bea na<Esc>hxpasr<Esc>iwe<Esc>A b<Esc>3aan<Esc>x`得到答案，并能够解释其中操作含义
	- vim常用命令

###文件、目录管理
- 时间：一周
- 技能点：用户组、权限问题
- 考核内容：
	- 根据不同的要求能够找到符合条件的文件

###文件系统管理
- 时间：一周
- 技能点：文件系统相关操作
- 考核内容：
	- 磁盘分区、格式化、检验、挂载
	- 常见压缩命令（gzip、zcat）
	- 常见打包命令（tar）

###bash的环境配置及操作环境
- 时间：一周
- 技能点：变量的增改及替换、数据流重定向、管道命令
- 考核内容：
	- 选取命令、排序命令、切割命令考核等

###正则表达式、文件格式化处理
- 时间：两周
- 技能点：正则、grep参数、sed、awk
- 考核内容：
	- 通过grep查找特殊字符串并配合重定向处理大量的文件查找问题
	- 创建一个新的命令达到特别的目的
	- 将一个文本去除空白行、开头为X的行

###安全相关
- 时间：一周
- 技能点：熟悉配置iptables、firewalld
- 考核内容：
	- iptables语法格式
	- firewalld-cmd命令

###搭建网络服务
- 时间：两周
- 技能点：**熟悉LNMP**、NFS文件共享、nginx服务器、SSL应用、MySQl基础
- 考核内容：
	- 环境搭建、比较熟悉nginx
	- 创建证书
	- HTTP响应状态码（熟记常见的状态码）
	- 利用MySQl创建数据库和数据表并进行相关操作

###搭建Discuz！
- 时间：一周
- 技能点：LAMP或LNMP的应用
- 考核内容：搭建是否成功、熟悉其管理界面
- 进阶：[Discuz开发手册](http://discuzt.cr180.com/discuzcode-dir_index.html)

###搭建WordPress
- 时间：一周
- 技能点：熟悉环境、添加站点
- 考核内容：搭建是否成功

   
   
   