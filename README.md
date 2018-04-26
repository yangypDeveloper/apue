# UNIX高级环境编程(第三版)环境搭建

> 环境安装:环境,云服务器 CentOS Linux release 7.4.1708 (Core) ,本地 macOS 10.13

## 0. 相关网站 
[apue第三版官方网站](http://www.apuebook.com/apue3e.html)

可以下载源码

## 1. 源码下载

```
wget http://www.apuebook.com/src.3e.tar.gz
tar -zxvf src.3e.tar.gz 
```

## 2. 编译
进入解压后的文件夹执行命令: `make`

## 3. 报错

> undefined reference to 'heapsort' collect2: error: ld returned 1 exit status

## 4. 解决

```
wget http://elrepo.reloumirrors.net/testing/el6/x86_64/RPMS/libbsd-0.2.0-4.el6.elrepo.x86_64.rpm
wget http://elrepo.reloumirrors.net/testing/el6/x86_64/RPMS/libbsd-devel-0.2.0-4.el6.elrepo.x86_64.rpm

rpm -ivh libbsd-0.2.0-4.el6.elrepo.x86_64.rpm
rpm -ivh libbsd-devel-0.2.0-4.el6.elrepo.x86_64.rpm  

sudo cp  ./include/apue.h   /usr/include/
sudo cp  ./lib/libapue.a   /usr/local/lib/
```
## 5. 测试

> 网上买的书还没到,先跟着网上的中文版的做,报错中对照英文版才发现第二版,和第三版有些出入,

example 1.3 ls1.c

```c
#include "apue.h"
#include <dirent.h>

int 
main(int argc ,char *argv[])
{
    DIR *dp;
    struct dirent *dirp;

    if(argc != 2)
        err_quit("a single argument (thisdirectory name)is required");

    if((dp = opendir(argv[1])) == NULL)
        err_sys("can't open %s",argv[1]);
    while ((dirp = readdir(dp)) != NULL)
        printf("%s\n",dirp->d_name);

    closedir(dp);
    exit(0);
    return 0;
}
```

## 6. 再报错

> undefined reference to `err_quit'

没有找到err_quit的实现,指定apue库即可

```
gcc ls1.c -lapue
```



## 7. 成功

```
# ./ls1
a single argument (thisdirectory name)is required
# ./ls1 a
can't open a: No such file or directory
# ./ls1 ./
..
apue.3e
ls1.c
src.3e.tar.gz
.
ls1
```

## 8. hello world

```c
#include "apue.h"

int
main(void)
{
	printf("hello world from process ID %ld\n", (long)getpid());
	exit(0);
}

```

## 9. 结束
> hello.c和ls1.c源代码都可以在apue.3e中intro中找到