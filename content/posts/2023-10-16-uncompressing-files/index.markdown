---
title: Uncompressing files
author: ''
date: '2023-10-16'
excerpt: ""
categories: ["bash"]
tags: []
---

I always forget which codes to use to uncompress different compressed file types. 

This [post](https://unix.stackexchange.com/questions/94837/having-trouble-uncompressing-a-few-files) was really helpful.


```bash
file file.zip 
# file.zip: Zip archive data, at least v1.0 to extract
# solution: use unzip.

file file.rar 
# file.rar: RAR archive data, v1d, os: Win32
# solution: use unrar.

file file.7z 
# file.7z: 7-zip archive data, version 0.3
# solution: use 7z.

file file.tgz 
# file.tgz: gzip compressed data, from Unix, last modified: Sun Oct 13 01:14:43 2013
# solution: use tar zxvf.

file file.tar.bz2 
# file.tar.bz2: bzip2 compressed data, block size = 900k
# solution: use tar jxvf.

file afile.gz 
# file.gz: gzip compressed data, was "afile", from Unix, last modified: Sun Oct 13 01:10:19 2013
# solution: use gunzip.

file file.tar
# file.tar: POSIX tar archive
# solution: use tar xvf file.tar
```
