### 超微主板安装CentOS 6无法进入图形界面的解决方案

进入安装选择界面时，有以下选项：
```
Install or upgrade an existing system
Install system with basic video driver
Rescue installed system
Boot from local drive
Memory test
```

选择第一个选项，然后按Tab键，输入“nomodeset blacklist=ast xdriver=vesa brokenmodules=ast”，全文如下所示：
```
vmlinuz initrd=initrd.img nomodeset blacklist=ast xdriver=vesa brokenmodules=ast
```
