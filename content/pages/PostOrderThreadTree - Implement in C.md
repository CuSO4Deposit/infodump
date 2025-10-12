---
tags:
- C
- Data
- Structure
date: 2021-11-04
title: PostOrderThreadTree - Implement in C
categories:
lastMod: 2025-10-12
--- 
```cpp
/*
 * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
 *
 *       Filename:  ThrTree.cpp
 *
 *    Description:  A BiThrTree
 *
 *        Version:  1.0
 *        Created:  2021/11/3 22:50:46
 *       Revision:  none
 *       Compiler:  gcc
 *
 *         Author:  CuSO4_Deposit (Depoze), CuSO4D@protonmail.com
 */
#include <stdio.h>
#include <iostream>
#include <stdlib.h>
#include ".\BiTreeForThr.h"

#define TRUE 1
#define FALSE 0
#define SUCCESS 1
#define FAILURE 0

typedef int Status;
typedef char TElemtype;
typedef enum PointerTag{Child = 0, Thread = 1} PointerTag;
typedef struct BiThrNode{
    struct BiThrNode* lptr;
    PointerTag ltag;
    TElemtype data;
    struct BiThrNode* parent;
    PointerTag rtag;
    struct BiThrNode* rptr;
}BiThrNode;

BiThrNode* PostCopyTree(BiTNode* T){
    /* 
     * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
     *         Name:  PostCopyTree
     *  Description:  Copy a Bitree T to a ThrTree in post order return the BiThrtree's root.
     * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
     */
    if(!T)  return NULL;
    BiThrNode* newlp = PostCopyTree(T->lchild);
    BiThrNode* newrp = PostCopyTree(T->rchild);
    BiThrNode* newnode = (BiThrNode*)malloc(sizeof(BiThrNode));
    newnode->data = T->data;
    newnode->lptr = newlp;
    newnode->rptr = newrp;
    newnode->ltag = Child;
    newnode->rtag = Child;
    newnode->parent = NULL;
    newlp->parent = newnode;
    newrp->parent = newnode;
    return newnode;
}

Status PostThreading(BiThrNode* ThrT, BiThrNode*& Pre){
    /* 
     * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
     *         Name:  PostThreading
     *  Description: Initialize a PostOrderThreadTree. 
     *     Argument: Pre is a global varible with initial value NULL,
     * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
     */
    if(!ThrT)   return SUCCESS;
    PostThreading(ThrT->lptr, Pre);
    PostThreading(ThrT->rptr, Pre);
    if(ThrT->lptr)  ThrT->ltag = Child;
    else{
        ThrT->lptr = Pre;
        ThrT->ltag = Thread;
    }
    if(Pre){
        if(Pre->rptr)   Pre->rtag = Child;
        else{
            Pre->rtag = Thread;
            Pre->rptr = ThrT;
        }
    }
    Pre = ThrT;
    return SUCCESS;

}

BiThrNode* LeftDeepest(BiThrNode* T){
    /* 
     * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
     *         Name:  LeftDeepest
     *  Description:  return the leftmost deepest leaf node of T.
     * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
     */
    if(T->ltag {{< logseq/mark >}} Thread && T->rtag {{< / logseq/mark >}} Thread)  return T;
    if(T->ltag == Thread)   return LeftDeepest(T->rptr);
    return LeftDeepest(T->lptr);
}

BiThrNode* PostOrderPre(BiThrNode* ThrT){
    /* 
     * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
     *         Name:  PostOrderPre
     *  Description: return the previous one of T in PostOrder Sequence. 
     * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
     */
    if(ThrT->ltag == Thread)    return ThrT->lptr;
    if(ThrT->rptr)  return ThrT->rptr;
    else return ThrT->lptr;
}

BiThrNode* PostOrderNext(BiThrNode* ThrT){
    /* 
     * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
     *         Name:  PostOrderNext
     *  Description: return the next one of T in PostOrder Sequence. 
     * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
     */
    if(ThrT->parent == NULL)    return NULL;
    if(ThrT->rtag == 1) return ThrT->rptr;
    if(ThrT == ThrT->parent->lptr)
        if(ThrT->parent->rtag == Thread)    return ThrT->parent;
        else return LeftDeepest(ThrT->parent->rptr);
    else return ThrT->parent;
}

Status TestVisit(BiThrNode* ThrT){
    printf("%c\n", ThrT->data);
    return SUCCESS;
}

Status PostThrTreeTraverse(BiThrNode* ThrT, Status (*visit)(BiThrNode*)){
    /* 
     * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
     *         Name:  PostThrTreeTraverse
     *  Description:  Traverse a PostOrderThreadTree.
     * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
     */
    BiThrNode* TraversePtr = LeftDeepest(ThrT);
    while(TraversePtr){
        visit(TraversePtr);
        TraversePtr = PostOrderNext(TraversePtr);
    }
    return SUCCESS;
}

int main(){
    BiTNode* T = (BiTNode*)malloc(sizeof(BiTNode));
    InitBiTree(T);
    CreateBiTree(T);
    BiThrNode* Pre = NULL;
    BiThrNode* ThrT = PostCopyTree(T);
    PostThreading(ThrT, Pre);
    PostThrTreeTraverse(ThrT, TestVisit);
    return 0;
}
```
 