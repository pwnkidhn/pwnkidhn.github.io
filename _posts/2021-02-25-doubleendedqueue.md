---
layout: post
title: "Double ended Queue "
date: 2021-02-25
categories: DataStructure
tags: [Data Structure, Double ended Queue, Queue]
---

<center>Basic code of Double ended Queue</center> 

## Abstract Data Type
```c
typedef DLDeque Deque;

void DequeInit(Deque* pdeq);
- Initialize the Queue. should be called at first. 
int DQIsEmpty(Deque* pdeq);
- if queue is empty return TRUE, else FALSE.
void DQAddFirst(Deque* pdeq, Data data);
- Input data into head of Queue.
void DQAddLast(Deque* pdeq, Data data);
- Input data into tail of Queue.
Data DQRemoveFirst(Deque* pdeq);
- Output data from head of Queue also remove the data.
Data DQRemoveLast(Deque* pdeq);
- Output data from tail of Queue also remove the data.
Data DQGetFirst(Deque* pdeq);
- Output data from head of Queue.
Data DQGetLast(Deque* pdeq);
- Output data from tail of Queue.
```


## Declaration
```c
#include<stdlib.h>
#include"Deque.h"

void DequeInit(Deque* pdeq) {
	pdeq->head = NULL;
	pdeq->tail = NULL;
}
int DQIsEmpty(Deque* pdeq) {
	if (pdeq->head == NULL)
		return TRUE;
	else
		return FALSE;
}
void DQAddFirst(Deque* pdeq, Data data) {
	Node* newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;
	newNode->next = pdeq->head;
	if (DQIsEmpty(pdeq)) {
		pdeq->tail = newNode;
	}
	else {
		pdeq->head->prev = newNode;
	}
	newNode->prev = NULL;
	pdeq->head = newNode;
}
void DQAddLast(Deque* pdeq, Data data) {
	Node* newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;
	newNode->prev = pdeq->tail;
	if (DQIsEmpty(pdeq)) {
		pdeq->head = newNode;
	}
	else {
		pdeq->tail->next = newNode;
	}
	newNode->next = NULL;
	pdeq->tail = newNode;
}
Data DQRemoveFirst(Deque* pdeq) {
	Node* rpos = pdeq->head;
	Data rdata;
	if (DQIsEmpty(pdeq)) {
		exit(-1);
	}
	rdata = rpos->data;

	pdeq->head = pdeq->head->next;
	free(rpos);

	if (pdeq->head == NULL)
		pdeq->tail == NULL;
	else
		pdeq->head->prev = NULL;
	return rdata;
}
Data DQRemoveLast(Deque* pdeq) {
	Node* rpos = pdeq->tail;
	Data rdata;
	if (DQIsEmpty(pdeq)) {
		exit(-1);
	}
	rdata = rpos->data;
	
	pdeq->tail = pdeq->tail->prev;
	free(rpos);

	if (pdeq->tail == NULL)
		pdeq->head = NULL;
	else
		pdeq->tail->next = NULL;
	return rdata;
}
Data DQGetFirst(Deque* pdeq) {
	if (DQIsEmpty(pdeq)) {
		exit(-1);
	}
	return pdeq->head->data;
}
Data DQGetLast(Deque* pdeq) {
	if (DQIsEmpty(pdeq)) {
		exit(-1);
	}
	return pdeq->tail->data;
}
```

## Example Code
```c
#include"Deque.h"
#include<stdio.h>
int main() {
	Deque dq;
	DequeInit(&dq);

	DQAddFirst(&dq, 3);
	DQAddFirst(&dq, 2);
	DQAddFirst(&dq, 1);
	
	DQAddLast(&dq, 4);
	DQAddLast(&dq, 5);
	DQAddLast(&dq, 6);

	while (!DQIsEmpty(&dq)) {
		printf("%d ", DQRemoveFirst(&dq));
	}

	printf("\n");
	DQAddFirst(&dq, 3);
	DQAddFirst(&dq, 2);
	DQAddFirst(&dq, 1);

	DQAddLast(&dq, 4);
	DQAddLast(&dq, 5);
	DQAddLast(&dq, 6);

	while (!DQIsEmpty(&dq)) {
		printf("%d ", DQRemoveLast(&dq));
	}
	return 0;
}
```
