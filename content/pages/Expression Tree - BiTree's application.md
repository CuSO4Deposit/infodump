---
tags:
- C
- Data
- Structure
date: 2021-11-12
title: Expression Tree - BiTree's application
categories:
lastMod: 2025-10-12
--- 
## Introduction

An expression can be converted to a tree, when every operator in the expression is binocular. One who is familiar with Mathematica may also know Mathematica expressions are all pre-order expressions.

![](/images/2021/图片-1.png)

PreOrder expressions in mma

This cpp program is used to read a expression and convert to a tree in memory, and calculate its value by means of the tree.
  
## Code

```cpp
#ifndef LINK_QUEUE_H
#define LINK_QUEUE_H
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  Link-Queue.h
*
*    Description: Implement a link queue in C. 
*
*        Version:  1.0
*        Created:  2021/10/20 23:22:12
*       Compiler:  gcc
*         Author:  CuSO4_Deposit (Depoze), CuSO4D@protonmail.com
*/

#include <stdio.h>
#include <iostream>
#include <stdlib.h>

#define TRUE 1
#define FALSE 0
#define SUCCESS 1
#define FAILURE 0

typedef int Status;
typedef union{
  float Number;
  char Operator;
}OpElem;
typedef struct{
  bool tag;
  OpElem Elem;
}QElemtype;
typedef struct QNode{
  QElemtype data;
  struct QNode* next;
}QNode;
typedef struct{
  QNode* front;
  QNode* rear;
}LinkQueue;

// Function Declaration
Status InitQueue(LinkQueue& Q);
Status DestroyQueue(LinkQueue& Q);
Status ClearQueue(LinkQueue& Q);
bool QueueEmpty(LinkQueue Q);
int QueueLength(LinkQueue Q);
Status GetHead(LinkQueue Q, QElemtype& e);
Status EnQueue(LinkQueue& Q, QElemtype e);
Status DeQueue(LinkQueue& Q, QElemtype& e);
Status QueueTraverse(LinkQueue Q, Status (*Visit)(QNode*));
Status TestTraverse(QNode* n);

// Function Implement

Status InitQueue(LinkQueue& Q){
 /* 
  * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
  *         Name:  InitQueue
  *  Description:  Initialize a Queue. Will allocate memory for pointers.
  * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
  */
  Q.front = (QNode*)malloc(sizeof(QNode));
  if(!Q.front)    return FAILURE;
  Q.rear = Q.front;
  return SUCCESS;
}    

Status DestroyQueue(LinkQueue& Q) {
  /*
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  DestroyQueue
   *  Description:  Destroy a Queue. Free all the pointers.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if (!ClearQueue(Q))  return FAILURE;
  free(Q.rear);   Q.rear = NULL;  //Q.rear and Q.front must point to the 
  Q.front = NULL;                 // same address, free once.        
  return SUCCESS;
}

Status ClearQueue(LinkQueue& Q){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  ClearQueue
   *  Description:  Clear Q to an emply queue.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  QElemtype temp;
  while(!QueueEmpty(Q))
      if(!DeQueue(Q, temp))   return FAILURE;
  return SUCCESS;
}

int QueueLength(LinkQueue Q){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  QueueLength
   *  Description:  return the number of nodes in Q.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  int counter = 0;
  QNode* cur = Q.front;
  while((cur = cur->next))
      counter++;
  return counter;
}

bool QueueEmpty(LinkQueue Q){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  QueueEmpty
   *  Description:  return TRUE if Q is empty; else return FALSE.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(Q.front->next == NULL)   return TRUE;
  else return FALSE;
}

Status GetHead(LinkQueue Q, QElemtype& e){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  GetHead
   *  Description:  return the first element in Q with e.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(QueueEmpty(Q))   return FAILURE;
  e = (Q.front->next)->data;
  return SUCCESS;
}

Status EnQueue(LinkQueue& Q, QElemtype e){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  EnQueue
   *  Description:  insert e to Queue ('s rear).
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  QNode* NewNodePtr = (QNode*)malloc(sizeof(QNode));
  if(!NewNodePtr) return FAILURE;
  NewNodePtr -> data = e;
  NewNodePtr -> next = NULL;
  Q.rear -> next = NewNodePtr;
  Q.rear = NewNodePtr;
  return SUCCESS;
}    

Status DeQueue(LinkQueue& Q, QElemtype& e) {
  /*
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  DeQueue
   *  Description:  Delete the first element in Q after assigning it to e.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if (QueueEmpty(Q))   return FAILURE;
  e = (Q.front->next)->data;
  QNode* FreeHere = Q.front->next;
  if (FreeHere == Q.rear)  Q.rear = Q.front;
  Q.front->next = FreeHere->next;
  free(FreeHere); FreeHere = NULL;
  return SUCCESS;
}

Status QueueTraverse(LinkQueue Q, Status (*Visit(QNode*))){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  QueueTraverse
   *  Description:  Traverse Q. To each QNode n, do "Visit(&n);".
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  QNode* TraversePtr = Q.front;
  while((TraversePtr = TraversePtr -> next))
      if(!Visit(TraversePtr)) return FAILURE;
  return SUCCESS;
}

Status TestTraverse(QNode* n){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  TestTraverse
   *  Description:  a test function for QueueTraverse().
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  printf("%c\n", n->data);
  return SUCCESS;
}

//suggest parentheses around assignment used as truth value/
#endif //#ifndef LINK_QUEUE_H

```

```cpp
#ifndef SEQUENCE_STACK_H
#define SEQUENCE_STACK_H
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  Sequence Stack.h
*
*    Description: Implement sequence stack in C. 
*
*        Version:  1.0
*        Created:  2021/9/29 23:31:18
*       Revision:  none
*       Compiler:  gcc
*
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
typedef float SElemtype;
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
*(S.top++) = e;
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
#endif //#ifndef SEQUENCE_STACK_H
```

```cpp
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  BiTree.h
*
*    Description:  Implemrnt a BiTree in C.
*
*        Version:  1.0
*        Created:  2021/10/21 18:21:42
*       Revision:  none
*       Compiler:  gcc
*
*         Author:  CuSO4_Deposit (Depoze), CuSO4D@protonmail.com
*/         
#ifndef BITREE_H
#define BITREE_H

#include <stdio.h>
#include <iostream>
#include <stdlib.h>
#define TRUE 1
#define FALSE 0
#define SUCCESS 1
#define FAILURE 0

typedef int Status;
typedef char TElemtype;
typedef struct BiTNode{
  TElemtype data;
  struct BiTNode* lchild;
  struct BiTNode* rchild;
}BiTNode;

typedef BiTNode* QElemtype; 
typedef struct QNode{
  QElemtype data;
  struct QNode* next;
}QNode;
typedef struct{
  QNode* front;
  QNode* rear;
}LinkQueue;

// Queue Function Declaration
Status InitQueue(LinkQueue& Q);
Status DestroyQueue(LinkQueue& Q);
bool QueueEmpty(LinkQueue Q);
Status ClearQueue(LinkQueue& Q);
Status EnQueue(LinkQueue& Q, QElemtype e);
Status DeQueue(LinkQueue& Q, QElemtype& e);

// Function Declaration
Status InitBiTree(BiTNode* T);
Status DestroyBiTree(BiTNode*& T); 
Status ClearBiTree(BiTNode*& T);
Status CreateBiTree(BiTNode*& T);
bool BiTreeEmpty(BiTNode* T);
int BiTreeDepth(BiTNode* T);
TElemtype Value(BiTNode* T);
Status Assign(BiTNode* T, TElemtype e);
BiTNode* Parent(BiTNode* T);
BiTNode* LeftChild(BiTNode* T);
BiTNode* RightChild(BiTNode* T);
Status InsertChild(BiTNode* T, int LR, TElemtype Value);
Status DeleteChild(BiTNode*& T, int LR);
Status PostorderTraverse(BiTNode* T, Status (*visit)(BiTNode*));
Status LevelorderTraverse(BiTNode* T, Status (*visit)(BiTNode*));

Status InitQueue(LinkQueue& Q){
 /* 
  * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
  *         Name:  InitQueue
  *  Description:  Initialize a Queue. Will allocate memory for pointers.
  * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
  */
  Q.front = (QNode*)malloc(sizeof(QNode));
  if(!Q.front)    return FAILURE;
  Q.front->next = NULL;
  Q.rear = Q.front;
  return SUCCESS;
}    

Status DestroyQueue(LinkQueue& Q){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  DestroyQueue
   *  Description:  Destroy a Queue. Free all the pointers.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(!ClearQueue(Q))  return FAILURE;
  free(Q.rear);   Q.rear = NULL;  //Q.rear and Q.front must point to the 
  Q.front = NULL;                 // same address, free once.        
  return SUCCESS;
}

Status ClearQueue(LinkQueue& Q){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  ClearQueue
   *  Description:  Clear Q to an emply queue.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  QElemtype temp;
  while(!QueueEmpty(Q))
      if(!DeQueue(Q, temp))   return FAILURE;
  return SUCCESS;
}

Status EnQueue(LinkQueue& Q, QElemtype e){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  EnQueue
   *  Description:  insert e to Queue ('s rear).
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  QNode* NewNodePtr = (QNode*)malloc(sizeof(QNode));
  if(!NewNodePtr) return FAILURE;
  NewNodePtr -> data = e;
  NewNodePtr -> next = NULL;
  Q.rear -> next = NewNodePtr;
  Q.rear = NewNodePtr;
  return SUCCESS;
}    

Status DeQueue(LinkQueue& Q, QElemtype& e){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  DeQueue
   *  Description:  Delete the first element in Q after assigning it to e.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(QueueEmpty(Q))   return FAILURE;
  e = (Q.front -> next) -> data;
  QNode* FreeHere = Q.front -> next;
  if (FreeHere == Q.rear)  Q.rear = Q.front;
  Q.front -> next = FreeHere -> next;
  free(FreeHere); FreeHere = NULL;
  return SUCCESS;
}

bool QueueEmpty(LinkQueue Q){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  QueueEmpty
   *  Description:  return TRUE if Q is empty; else return FALSE.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(Q.front->next == NULL)   return TRUE;
  else return FALSE;
}
Status InitBiTree(BiTNode* T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  InitBiTree
   *  Description:  Initialize a BiTree, assigning NULL to pointers.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  T->lchild = NULL;
  T->rchild = NULL;
  return SUCCESS;
}

Status DestroyBiTree(BiTNode*& T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  DestroyBiTree
   *  Description:  free memory.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(!T) return SUCCESS;
  DestroyBiTree(T->lchild);
  DestroyBiTree(T->rchild);
  free(T); T = NULL;
  return SUCCESS;
}

Status PostorderTraverse(BiTNode* T, Status (*visit)(BiTNode*)){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  PostorderTraverse
   *  Description:  Postorfer traverse a tree. to each node N. execute visit(N);
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(!T)  return SUCCESS;
  PostorderTraverse(T->lchild, visit);
  PostorderTraverse(T->rchild, visit);
  visit(T);
  return SUCCESS;
}

Status ClearBiTree(BiTNode*& T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  ClearBiTree
   *  Description:  Delete all son nodes, T becomes an empty tree.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(!T) return SUCCESS;
  ClearBiTree(T->lchild);
  ClearBiTree(T->rchild);
  DeleteChild(T, 0);
  DeleteChild(T, 1);
  return SUCCESS;
}

Status CreateBiTree(BiTNode*& T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  CreateBiTree
   *  Description:  input a preorder traverse sequence, replace empty tree with '#'.
   *                  Function will create a BiTree with T as its root.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  char temp = '0';
  std::cin>>temp;
  if (temp == '#'){
      T = NULL;
      return SUCCESS;
  }
  else{
      if(!(T = (BiTNode*)malloc(sizeof(BiTNode)))) return FAILURE;
      T->data = temp;
      CreateBiTree(T->lchild);
      CreateBiTree(T->rchild);
  }
  return SUCCESS; //Invalid. just for fixing warning.
}

TElemtype Value(BiTNode* T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  Value
   *  Description:  return the data of T.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  return T->data;
}

Status Assign(BiTNode* T, TElemtype e){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  Assign
   *  Description:  assign e to T->data.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(!T)  return FAILURE;
  T->data = e;
  return SUCCESS;
}

Status DeleteChild(BiTNode*& T, int LR){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  DeleteChlid
   *  Description:  Delete the child of a node. L = 0; R = 1.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(LR){
      if(!T->rchild)  return SUCCESS;
      free(T->rchild);    T->rchild = NULL;
  }
  else{   //LR = 1
      if(!T->lchild)  return SUCCESS;
      free(T->lchild);    T->lchild = NULL; 
  }
  return SUCCESS;
}

Status InsertChild(BiTNode* T, int LR, TElemtype ChildData){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  InsertChild
   *  Description:  insert a child to this root. L = 0, R = 1.
   *                  if the place has already got a child, return FALSE. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(LR){
      if(T->rchild)   return FAILURE;
      BiTNode* temp = (BiTNode*)malloc(sizeof(BiTNode));
      temp->data = ChildData;
      temp->lchild = NULL; temp->rchild = NULL;
      T->rchild = temp;
  }
  else{   //LR = 1
      if(T->lchild)   return FAILURE;
      BiTNode* temp = (BiTNode*)malloc(sizeof(BiTNode));
      temp->data = ChildData;
      temp->lchild = NULL; temp->rchild = NULL;
      T->lchild = temp;
  }
  return SUCCESS;
}

bool BiTreeEmpty(BiTNode* T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  BiTreeEmpty
   *  Description:  if BiTree is empty, return TRUE, else return FALSE.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
 if(T)    return TRUE;
 else return FALSE;
}

int BiTreeDepth(BiTNode* T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  BiTreeDepth
   *  Description:  return the depth of bitree. (the depth of only a root is 1)
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(!T)  return 0;
  int lDepth = BiTreeDepth(T->lchild);
  int rDepth = BiTreeDepth(T->rchild);
  return lDepth < rDepth
      ? rDepth + 1
      : lDepth + 1;
}

Status LevelorderTraverse(BiTNode* T, Status (*visit)(BiTNode*)){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  LevelorderTraverse
   *  Description:  Traverse in level order, based on queue.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  LinkQueue Q;
  InitQueue(Q);
  QElemtype Processing;
  if(!T)  return SUCCESS;
  EnQueue(Q, T);
  while (!QueueEmpty(Q)){
      DeQueue(Q, Processing);
      if(!visit(Processing))  return FAILURE;
      if(Processing->lchild)  EnQueue(Q, Processing->lchild);
      if(Processing->rchild)  EnQueue(Q, Processing->rchild);
  }

  DestroyQueue(Q);
  return SUCCESS;
}

BiTNode* Parent(BiTNode* T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  Parent
   *  Description:  if T has parent return its parent, else return NULL.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  LinkQueue Q;
  InitQueue(Q);
  QElemtype Processing;
  while (!QueueEmpty(Q)){
      DeQueue(Q, Processing);
      if(Processing->lchild {{< logseq/mark >}} T  Processing->rchild {{< / logseq/mark >}} T)  return Processing;
      if(Processing->lchild)  EnQueue(Q, Processing->lchild);
      if(Processing->rchild)  EnQueue(Q, Processing->rchild);
  }
  return NULL;
  DestroyQueue(Q);

}

BiTNode* LeftChild(BiTNode* T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  LeftChild
   *  Description:  return the address of T's left child. if it doesn't have one, return NULL.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  return T->lchild;
}

BiTNode* RightChild(BiTNode* T){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  RightChild
   *  Description:  return the address of T's right child. if it doesn't have one, return NULL.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  return T->rchild;
}

#endif  //  #ifndef BITREE_H 


```

```cpp
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  ExpressionTree.cpp
*
*    Description:  Using tree structure to calculate expressions.
*
*        Version:  1.0
*        Created:  2021/11/4 23:13:44
*       Revision:  none
*       Compiler:  gcc
*
*         Author:  CuSO4_Deposit (Depoze), CuSO4D@protonmail.com
*/
#include <stdio.h>
#include <iostream>
#include <stdlib.h>
#include ".\BiTree.h"
#include ".\Link-QueueForExp.h"
#include ".\Sequence-StackForExp.h"

Status Expression2Queue(LinkQueue& Q){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  InputData
   *  Description:  input a unit of expression, this function will recognize its 
   *                  type, tag it and insert it into queue Q.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  bool flag = 0;
  float num = 0; float dec = 1; bool point = 0;
  char* expression = (char*)malloc(50 * sizeof(char));
//  std::cin >> expression;  // cin automatically ends input when reads <space>.
  fgets(expression, 49, stdin);
  char* charptr = expression;
  while(!flag){
      while(*(charptr)){
          if(*charptr == '#'){
              flag = 1;
              break;
          }
          if(
                  *charptr == '+' 
                  *charptr == '-' 
                  *charptr == '*' 
                  *charptr == '/' 
            ){
              QElemtype Processing;
              Processing.tag = 0;
              Processing.Elem.Operator = *charptr;
              EnQueue(Q, Processing);
              charptr++;  break;
          }
          if(*charptr >= '0' && *charptr <= '9' && point == 0){
              num *= 10;
              num += (*charptr - 0x30);
          }
          if(*charptr == '.') point = 1;
          if(*charptr >= '0' && *charptr <= '9' && point == 1){
              dec /= 10;
              num += (*charptr - 0x30) * dec;
          }
          if(charptr[1] < '0'  charptr[1] > '9')
              if((point {{< logseq/mark >}} 1)  (point {{< / logseq/mark >}} 0 && charptr[1] != '.')){
                  QElemtype Processing;
                  Processing.tag = 1;
                  Processing.Elem.Number = num;
                  EnQueue(Q, Processing);
                  num = 0; dec = 1; point = 0;
                  charptr++; charptr++; break;    //skip the space behind the number
              }
          charptr++;
      }
  }
  return SUCCESS;
}

Status CalculateVisit(BiTNode* T, SqStack& S) {
  if (T->data->tag == 0) {
      float num = 0;
      float Operand1 = 0; float Operand2 = 0;
      Pop(S, Operand1);   Pop(S, Operand2);
      switch (T->data->Elem.Operator)
      {
      case '+':   num = Operand1 + Operand2;  break;
      case '-':   num = Operand2 - Operand1;  break;
      case '*':   num = Operand1 * Operand2;  break;
      case '/':   if (Operand2 == 0)   return FAILURE;
                  else num = Operand2 / Operand1;  break;
      default:    return 2;
          break;
      }
      Push(S, num);
      return SUCCESS;
  }
  else {// T->data->tag == 1
      Push(S, T->data->Elem.Number);
      return SUCCESS;
  }
}

float CalculateExpTree(BiTNode* T) {
  if (!T) return 0;
  SqStack S;
  InitStack(S);
  PostorderTraverse(T, S, CalculateVisit);
  float result = 0; Pop(S, result);
  return result;
}

int main(){
  LinkQueue Q;
  InitQueue(Q);
  Expression2Queue(Q);
  BiTNode* T = (BiTNode*)malloc(sizeof(BiTNode));
  InitBiTree(T);
  CreateBiTree(T, Q);
  printf("%f\n", CalculateExpTree(T));
  return 0;
}
```
  
## Test

input:

\+ 8 \* 21.3 - 10 8

Output:

C:\\WINDOWS\\system32\\cmd.exe /c (^"D:\\Learning\\Computer\\Data-Structure\\Lab\\BiTree&Application\\ExpressionTree.exe^" )
+ 8 \* 21.3 - 10 8 #
50.599998
Hit any key to close this window...
 