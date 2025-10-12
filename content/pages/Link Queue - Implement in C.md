---
tags:
- C
- Data
- Structure
date: 2021-10-20
title: Link Queue - Implement in C
categories:
lastMod: 2025-10-12
--- 
## Code

```cpp
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  Link-Queue.cpp
*
*    Description: Implement a link queue in C. 
*
*        Version:  1.1
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
typedef char QElemtype;
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

int main(){
  LinkQueue Q; 
  InitQueue(Q);
  EnQueue(Q, '0');
  EnQueue(Q, '1');
  printf("%d\n", QueueLength(Q));
  printf("%d\n", QueueEmpty(Q));
  QElemtype e = 0;
  DeQueue(Q, e);
  GetHead(Q, e);
  printf("%c\n", e);
  EnQueue(Q, '2');
  ClearQueue(Q);
  printf("%d\n", QueueEmpty(Q));
  DestroyQueue(Q);
  return 0;
}
//suggest parentheses around assignment used as truth value/
```
  
## Test Output

C:\\WINDOWS\\system32\\cmd.exe /c (^"D:\\Learning\\Computer\\Data-Structure\\ADT\\Link-Queue\\Link-Queue.exe^" )
2
0
1
1
Hit any key to close this window... [](C:\WINDOWS\system32\cmd.exe /c (^"D:\Learning\Computer\Data-Structure\ADT\Link-Queue\Link-Queue.exe^" )
2
0
1
1
Hit any key to close this window...) 
  
## Draft

[LinkQueue-Draft](https://depoze.uden.ai/wp-content/uploads/2021/10/LinkQueue-Draft.pdf)[Download](https://depoze.uden.ai/wp-content/uploads/2021/10/LinkQueue-Draft.pdf)
 