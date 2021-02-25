---
layout: post
title: "List base Circular Queue "
date: 2021-02-25
categories: DataStructure
tags: [Data Structure, List base Circular Queue, Queue]
---

<center>Basic code of List base Circular Queue</center> 

## Abstract Data Type
```c
typedef LQueue Queue;

void QueueInit(Queue* pq);
- Initialize the Queue. should be called at first. 
int QIsEmpty(Queue* pq);
- if queue is empty return TRUE, else FALSE.
void Enqueue(Queue* pq, Data data);
- Input data into Queue.
Data Dequeue(Queue* pq);
- Output data from Queue also remove the data.
Data QPeek(Queue* pq);
- Output data from Queue. 
```


## Declaration
```c
#include<stdlib.h>
#include"ListBaseQueue.h"

void QueueInit(Queue* pq) {
	pq->front = NULL;
	pq->rear = NULL;
}
int QIsEmpty(Queue* pq) {
	if (pq->front == NULL)
		return TRUE;
	else
		return FALSE;
}
void Enqueue(Queue* pq, Data data) {
	Node* newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;
	newNode->next = NULL;

	if (QIsEmpty(pq)) {
		pq->front = newNode;
		pq->rear = newNode;
	}
	else {
		pq->rear->next = newNode;
		pq->rear = newNode;
	}
}
Data Dequeue(Queue* pq) {
	Node* rpos;
	Data rdata;
	if (QIsEmpty(pq)) {
		printf("Queue memory error");
		exit(-1);
	}
	rpos = pq->front;
	rdata = rpos->data;

	pq->front = pq->front->next;
	free(rpos);
	return rdata;

}
Data QPeek(Queue* pq) {
	if (QIsEmpty(pq)) {
		printf("Queue memory error");
		exit(-1);
	}
	return pq->front->data;
}
```

## Example Code
```c
#include<stdio.h>
#include"ListBaseQueue.h"
int main() {
	Queue q;
	QueueInit(&q);

	Enqueue(&q, 1);
	Enqueue(&q, 2);
	Enqueue(&q, 3);
	Enqueue(&q, 4);
	Enqueue(&q, 5);

	while (!QIsEmpty(&q)) {
		printf("%d ", Dequeue(&q));
	}
	return 0;
}
```
