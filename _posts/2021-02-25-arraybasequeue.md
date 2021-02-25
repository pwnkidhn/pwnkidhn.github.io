---
layout: post
title: "Array base Circular Queue "
date: 2021-02-25
categories: DataStructure
tags: [Data Structure, Array base Circular Queue, Queue]
---

<center>Basic code of Array base Circular Queue</center> 

## Abstract Data Type
```c
typedef CQueue Queue;

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
#include<stdio.h>
#include<stdlib.h>
#include"CircularQueue.h"

void QueueInit(Queue* pq) {
	pq->front = 0;
	pq->rear = 0;
}
int QIsEmpty(Queue* pq) {
	if (pq->front == pq->rear)
		return TRUE;
	else
		return FALSE;
}
int NextPosIdx(int pos) {
	if (pos == QUE_LEN - 1)
		return 0;
	else
		return pos + 1;
}
void Enqueue(Queue* pq, Data data) {
	if (NextPosIdx(pq->rear) == pq->front) {
		printf("queue memory error!");
		exit(-1);
	}
	
	pq->rear = NextPosIdx(pq->rear);
	pq->queArr[pq->rear] = data;
}
Data Dequeue(Queue* pq) {
	if (QIsEmpty(pq)) {
		printf("queue memory error!");
		exit(-1);
	}
	
	pq->front = NextPosIdx(pq->front);
	return pq->queArr[pq->front];
}
Data QPeek(Queue* pq) {
	if (QIsEmpty(pq)) {
		printf("queue memory error!");
		exit(-1);
	}
	return pq->queArr[NextPosIdx(pq->front)];
}
```

## Example Code
```c
#include"CircularQueue.h"
#include<stdio.h>
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
