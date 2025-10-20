---
tags:
- C
- Data
- Structure
date: 2021-09-18
title: Sequence List - Implement in C
categories:
lastMod: 2025-10-18
--- 
## Introduction

Sequence list is a commonly used data structure, the easiest linear list to implement.
  
## Implement

```cpp
/* 
* Sequence List ADT
* Author: CuSO4_Deposit
*/

#include <stdio.h>
#include <malloc.h>
#include <iostream>

#define TRUE 1
#define FALSE 0
#define SUCCESS 1
#define FAILURE 0
#define ERROR 0
#define INFEASIBLE -1

#define InitialSize 50
#define IncrementSize 10

typedef int Status;
typedef char Elemtype;
typedef struct{
Elemtype* ptr;
int length;
int listsize;
}Sqlist;

//Function Declaration:
Status InitList(Sqlist& L);
Status ClearList(Sqlist& L);
Status DestroyList(Sqlist& L);
int ListLength(Sqlist L);
Status ListEmpty(Sqlist L);
Status GetElem(Sqlist L, int i, Elemtype& e);
int LocateElem(Sqlist L, Elemtype e, int (*Compare)(Elemtype, Elemtype));
int Compare(Elemtype e1, Elemtype e2);
Status PriorElem(Sqlist L, Elemtype cur_e, Elemtype& pre_e);
Status NextElem(Sqlist L, Elemtype cur_e, Elemtype& next_e);
Status ListInsert(Sqlist& L, int i, Elemtype e); 
Status ListIncrease(Sqlist& L);
Status ListDelete(Sqlist& L, int i, Elemtype& e);
Status ListTraverse(Sqlist L, Status(*visit)(Elemtype));
Status Visit(Elemtype e);

//Function Implement:
Status InitList(Sqlist& L) {
/*
* Postcondition：Initialize a sqlist.
* Return value: 0 means failure. 1 means success.
*/
L.ptr = (Elemtype*)malloc(InitialSize * sizeof(Elemtype));
L.length = 0;
L.listsize = InitialSize;
if (L.ptr)return SUCCESS;
else return FAILURE;
}

Status ClearList(Sqlist& L) {
/*
* Postcondition: Clear a list to empty list.
*/
L.length = 0;
return SUCCESS;
}

Status DestroyList(Sqlist& L) {
/*
* Postcondition: Destroy a sqlist.
*/
ClearList(L);
free(L.ptr);
return SUCCESS;
}

int ListLength(Sqlist L) {
/*
* Return value: return the length of list.
*/
return L.length;
}

Status ListEmpty(Sqlist L) {
/*
* Return value: 0 means not empty. 1 means empty.
*/
if (L.length)return TRUE;
else return FALSE;
}

Status GetElem(Sqlist L, int i, Elemtype& e) {
/*
* Postcondition: e has the value of #i in the list L.
* Return value: 0 means failure. 1 means success.
*/
if (1 > i  i > L.length)return FAILURE;
e = L.ptr[i - 1];
return SUCCESS;
}

int LocateElem(Sqlist L, Elemtype e, int (*Compare)(Elemtype, Elemtype)) {
/*
* return value: 0:there's no such an elem; not 0: the order of the first elem. 
*/
int listlength = ListLength(L);
Elemtype temp;
for (int i = 1; i <= listlength; i++){
GetElem(L, i, temp);
if (Compare(e, temp))
return i;
}
return 0;
}

int Compare(Elemtype e1, Elemtype e2) {
/*
* An example of Compare function used in LocateElem function.
*/
return ((int)e1 - (int)e2);
}

Status PriorElem(Sqlist L, Elemtype cur_e, Elemtype& pre_e) {
/*
* return value: 0:failure; 1:success.
* Precondition: cur_e is in the list and not the 1st.
* Postcondition: Return the prior elem's value of the 1st cur_e with pre_e. 
*/
int listlength = ListLength(L);
for (int i = 1; i < listlength; i++){
if (L.ptr[i] == cur_e) {
pre_e = L.ptr[i - 1];
return SUCCESS;
}
}
return FAILURE;
}

Status NextElem(Sqlist L, Elemtype cur_e,Elemtype& next_e) {
/*
* return value: 0:failure; 1:success.
* Precondition: cur_e is in the list and not the last.
* Postcondition: Return the next elem's value of the 1st cur_e with next_e.
*/
int listlength = ListLength(L);
for (int i = 0; i < listlength - 1; i++){
if (L.ptr[i] == cur_e) {
next_e = L.ptr[i + 1];
return SUCCESS;
}
}
return FAILURE;
}

Status ListInsert(Sqlist& L, int i, Elemtype e) {
/*
* return value: 0:failure; 1:success.
* Precondition: 1 <= i <= ListLength(L) + １; List is not full.
* Postcondition: Insert e before the ith elem; Listlength++ .
*/
if (i < 1  i > ListLength(L) + 1)return FAILURE;
if (ListLength(L) == L.listsize)
if (!ListIncrease(L)) {
printf("Error when allocating space. ");
return FAILURE;
}
for (int j = ListLength(L) - 1; j >= i; j--)
L.ptr[j + 1] = L.ptr[j];
L.ptr[i] = e;
L.length++;
return SUCCESS;
}

Status ListIncrease(Sqlist& L) {
/*
* return value: 0:failure; 1:success.
* Precondition: L is full.
* Postcondition: reallocate for L.ptr. free L.ptr_legacy.
*/
Elemtype* Lnewptr = (Elemtype*)malloc((L.listsize + IncrementSize) * sizeof(Elemtype));
int listlength = ListLength(L);
if (!Lnewptr)return FAILURE;
for (int i = 0; i < listlength; i++)
Lnewptr[i] = L.ptr[i];
free(L.ptr);L.ptr = NULL;
L.ptr = Lnewptr;
L.listsize = L.listsize + IncrementSize;
return SUCCESS;
}

Status ListDelete(Sqlist& L, int i, Elemtype& e) {
/*
* return value: 0:failure; 1:success.
* Precondition: 1 <= i <= ListLength(L).
* Postcondition: Delete the ith elem, return its value with e.
*/
int listlength = ListLength(L);
if (i < 1  listlength < i)return FAILURE;
e = L.ptr[i - 1];
for (int j = i; j < listlength; j++){
L.ptr[j - 1] = L.ptr[j];
}
L.length--;
return SUCCESS;
}

Status ListTraverse(Sqlist L, Status (*visit)(Elemtype)) {
/*
* return value: 0:failure; 1: success;
* Postcondition: visit every elem.
*/
int listlength = ListLength(L);
for (int i = 0; i < listlength; i++){ 
if (!visit(L.ptr[i]))return FAILURE;
}
return SUCCESS;
}

Status Visit(Elemtype e) {
/*
* An example of Visit function used in ListTraverse Function.
*/
printf("%c\n", e);
return SUCCESS;
}

int main() {
Sqlist L;
int temp;
temp = InitList(L);
ClearList(L);
printf("%d", ListLength(L));
printf("%d", ListEmpty(L));
return 0;
}


```
 