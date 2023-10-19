---
title: Windows磁盘检查后found.xxx内.chk文件恢复
date: 2023-10-19 16:00:30
tags:
---
最近经常遇到磁盘错误，需要利用Windows的磁盘修复，具体操作为：右键需要恢复的磁盘分区->工具->检查
检查完成后发现分区根目录下出现`found.000`、`found.001`等文件夹，内部为一堆.chk文件。
参考[知乎文章](https://zhuanlan.zhihu.com/p/372662112)可知利用MIME可以获取到文件类型，以此猜测扩展名即可
脚本如下：
```python
# chk_restore.py
import magic
import mimetypes
import os
import glob
import sys
import shutil

source_path = sys.argv[1] 
target_path = sys.argv[2]

os.makedirs(target_path, exist_ok=True)

file_list = glob.glob(os.path.join(source_path, "*.chk"))
for f in file_list:
    f_mime = magic.from_file(f, mime=True) # 获取MIME
    ext_name = mimetypes.guess_extension(f_mime) # 根据MIME猜测扩展名
    _, full_name = os.path.split(f)
    f_name, _ = os.path.splitext(full_name)
    rec_file = os.path.join(target_path, f_name+ext_name)
    print(rec_file)
    shutil.copyfile(f, rec_file)
```
用法：
```bash
pip install python-magic # For Linux
pip install python-magic-bin # For Windows
python chk_restore.py $source_path $target_path # 填入源文件夹和恢复文件夹即可
```