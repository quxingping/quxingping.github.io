---
title: golang
tags:
  - golang
categories:
  - Golang
date: 2021-05-19 11:22:12
---

##### ***跨平台编译***

**linux 64**

```sql
SET CGO_ENABLED=0
SET GOOS=linux
SET GOARCH=amd64
go build 
```

**windows**

```sql
SET CGO_ENABLED=1
SET GOOS=windows
SET GOARCH=
go build    
```

##### panic错误重定向到日志文件

`https://zhuanlan.zhihu.com/p/245369778`

