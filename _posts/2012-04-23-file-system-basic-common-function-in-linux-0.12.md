iget() ----- 从设备dev上读取节点号为nr的i结点
1.从i结点表申请一个临时i结点
2.扫描i节点表，查找(设备号==dev&&i节点号==nr)的项
若未找到，则：
（1）用临时结点建立一个i节点
（2）从设备dev读取该i节点的信息
（3）返回该结点
若找到，等待i节点解锁
3.若(设备号!=dev||节点号!=nr)，则go to step 2
4.i节点引用计数增1
5.判断i节点的类型
若i节点是某个文件系统的安装点，则：
（1）在超级块中搜寻安装在此i节点的超级块
（2）将该i节点写盘
（3）从（1）的超级块上取设备号
（4）令i节点号为1
（5）重新扫描整个i节点表，获取被安装文件系统的根节点
若i节点不是其它文件系统的安装点，则：
（1）释放临时申请的i节点
（2）返回找到的i节点指针

iput ----- 释放i节点
1.若链接数为0，则释放所占用的磁盘块，释放i节点
2.把i节点的引用计数减1
3.判断i节点的类型
（1）管着：唤醒等待的进程
（2）块设备：刷新设备