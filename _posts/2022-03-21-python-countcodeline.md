---
layout: post
title:  "python-countcodeline"
category: python
tags: [python, fileline]
---
# python-countcodeline
*计算某个目录下特定文件的行数，全局变量进行配置参数，计算结果输出到文件指定文件：
主要的逻辑是遍历目录下的文件，然后读取文件中的内容，计算行数。
FilePath标识文件路径，patterns 匹配文件格式， single_level是否只搜索第一层文件。exceptFile指的是需要排除的目录，目的是将指定目录下特定的子目录不作为搜索计算的目录。outDir表示统计结果打印的输出文件。
代码中频繁调用的.rstrip('\n')目的是将文件中的 某一行的换行符去掉。
GetCommonElements目的是取两个集合的交集
computerLineNum计算某个文件有多少行，该函数利用了文件对象的enumerate轻松的将文本行数统计了出来。*
```
#-*- coding:utf-8 -*-
#计算目录下指定文件行数
import fnmatch, os
#要统计的文件夹地址
FilePath = 'D:\MAC2022'
#要统计的文件类型
patterns = '*.cpp;*.h;*.hpp;*.m;*.mm'
single_level = False
#需要排除的文件
exceptFile = 'build;Depend;Bin;debug;package;release;.gitignore;.git;.vs;jsoncpp;third_party;mars;TCore'
#统计结果输出文件地址
outDir = 'D:\MAC2022\output.txt'

def GetCommonElements(firstSet, secondSet):
    return set(firstSet).intersection(set(secondSet))

def allFiles(root, patterns = '*.cpp;*.h;*.hpp;*.m;*.mm', single_level = False,
             yield_folders = False, exceptFile = ""):
    patterns = patterns.split(';')
    excpetFiles = exceptFile.split(';')
    for path, subdirs, files in os.walk(root):
        lastDirs = path.split('\\')
        newSet = GetCommonElements(lastDirs, excpetFiles)
        if len(newSet) != 0:
            continue
        if yield_folders:
           #add subdirs to the tail of files
           files.extend(subdirs)
        files.sort()
        for name in files:
            for pattern in patterns:
                if fnmatch.fnmatch(name, pattern):
                    yield os.path.join(path, name)
                    break
        #only deal one level of the dir
        if single_level:
            break

def computerLineNum(strFileName):
    fileObj = open(strFileName, 'rU')
    count = 0
    for count, LineContent in enumerate(fileObj):
        pass
    return count + 1

def ComputerFilesTotalLineNumsInDir(dirName, outDir1):
    TotalCount = 0
    dirName = os.path.normpath(dirName)
    fileObj1 = open(outDir1, 'w')
    TotalFilecount = 0
    for name in allFiles(dirName, patterns = patterns,
                         exceptFile = exceptFile):
        count = computerLineNum(name)
        str1 = 'the line of the file %s is : %d\n'%(name, count)
        print(str1)
        fileObj1.write(str1)
        TotalCount += count
        TotalFilecount+=1
    str1 = 'the total line count of the dir %s is %d:'%(dirName, TotalCount)
    print(str1)
    fileObj1.write(str1)
    str1 = 'the total file count of the dir %s is %d:' % (dirName, TotalFilecount)
    print(str1)
    fileObj1.write(str1)
    fileObj1.close()

if __name__ == "__main__":
    ComputerFilesTotalLineNumsInDir(FilePath, outDir)
```
