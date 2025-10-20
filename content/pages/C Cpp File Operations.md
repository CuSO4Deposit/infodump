---
tags:
- C
date: 2021-10-05
title: C Cpp File Operations
categories:
lastMod: 2025-10-18
--- 
## C File Operation

VS do not allow "fopen" function to pass compilation.

```c
FILE* fp

//using MS Visual Studio(2019)
if ((fopen_s(&fp, "<path>", "<mode>"))){
printf("Cannot open the file.\n");    
exit(<error code>);
}

//using other compiler
if((fp=fopen("<path>", "mode")) == NULL ){
  printf("Fail to open the file.\n");
  exit(<error code>);
}
```
  
## C++ File Operation

[https://www.cplusplus.com/doc/tutorial/files/](https://www.cplusplus.com/doc/tutorial/files/)
 