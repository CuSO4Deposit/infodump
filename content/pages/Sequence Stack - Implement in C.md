---
tags:
- C
- Data
- Structure
date: 2021-09-29
title: Sequence Stack - Implement in C
categories:
lastMod: 2025-10-18
--- 
```cpp
/*
 * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
 *
 *       Filename:  Sequence Stack.cpp
 *
 *    Description: Implement sequence stack in C. 
 *
 *        Version:  1.0
 *        Created:  2021/9/29 23:31:18
 *       Revision:  none
 *       Compiler:  gcc
 *         Author:  CuSO4_Deposit (Depoze), CuSO4D@protonmail.com
*/
#include <stdio.h>
#include <stdlib.h>
#include <iostream>

#define TRUE 1
#define FALSE 0
#define SUCCESS 1
#define FAILURE 0

#define InitialSize 50
#define IncrementSize 10

typedef int Status;
typedef char SElemtype;
typedef struct {
SElemtype* base;
SElemtype* top;
int Stacksize;
}SqStack;

//Function Declaration
Status InitStack(SqStack& S);
Status DestroyStack(SqStack& S);
Status ClearStack(SqStack& S);
bool StackEmpty(SqStack S);
int StackLength(SqStack S);
Status GetTop(SqStack S, SElemtype& e);
Status StackIncrease(SqStack& S);
Status Push(SqStack& S, SElemtype e);
Status Pop(SqStack& S, SElemtype& e);
Status StackTraverse(SqStack S, Status(*Visit)(SElemtype*));
Status Visit(SElemtype* VisitPtr);

//Function Implement
Status InitStack(SqStack& S) {
/*
* Precondition: S exists.
* return value: 0:failure; 1:success.
*/
S.base = (SElemtype*)malloc(InitialSize * sizeof(SElemtype));
S.top = S.base;
S.Stacksize = InitialSize;
return SUCCESS;
}

Status DestroyStack(SqStack& S) {
/*
* Precondition: S exists.
* return value: 0:failure; 1:success.
*/
free(S.base);
S.top = NULL; S.base = NULL;
S.Stacksize = 0;
return SUCCESS;
}

Status ClearStack(SqStack& S) {
/*
* Precondition: S exists.
* return value: 0:failure; 1:success.
*/
S.top = S.base;
return SUCCESS;
}

bool StackEmpty(SqStack S) {
/*
* Precondition: S exists.
* return value: 0:false; 1:true.
*/
if (S.top == S.base)return TRUE;
else return FALSE;
}

int StackLength(SqStack S) {
/*
* Precondition: S exists.
* return value: the number of elem in S.
*/
return S.top - S.base;
}

Status GetTop(SqStack S, SElemtype& e) {
/*
* Precondition: S exists.
* Postcondition: return the top elem with e.
* return value: 0:failure; 1:success.
*/
e = *(S.top - 1);
return SUCCESS;
}

Status StackIncrease(SqStack& S) {
/*
* Precondition: S exists, S is full.
* return value: 0:failure; 1:success.
*/
SElemtype* newptr = (SElemtype*)realloc(S.base, (S.Stacksize + IncrementSize) * sizeof(SElemtype));
if (!newptr) { 
printf("Error reallocating space."); 
return FAILURE; 
}
S.base = newptr;
S.top = S.base + S.Stacksize;
S.Stacksize = S.Stacksize + IncrementSize;
return SUCCESS;
}

Status Push(SqStack& S, SElemtype e) {
/*
* Precondition: S exists.
* return value: 0:failure; 1:success.
*/
if (StackLength(S) == S.Stacksize)StackIncrease(S);
*S.top++ = e;
return SUCCESS;
}

Status Pop(SqStack& S, SElemtype& e) {
/*
* Precondition: S exists.
* return value: 0:failure; 1:success.
*/
e = *--S.top;
return SUCCESS;
}

Status StackTraverse(SqStack S, Status (*Visit)(SElemtype*)) {
/*
* Precondition: S exists.
* return value: 0:failure; 1:success.
*/
SElemtype* TraversePtr = S.base;
while (TraversePtr != S.top) 
if (!Visit(TraversePtr++))return FAILURE;
return SUCCESS;
}

Status Visit(SElemtype* VisitPtr) {
//An example of Visit function used in StackTraverse function.
printf("%c\n", *VisitPtr);
return SUCCESS;
}

//int main()
```
 