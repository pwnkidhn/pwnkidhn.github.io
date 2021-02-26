---
layout: post
title: "Heap for Priority queue"
date: 2021-02-26
categories: DataStructure
tags: [Data Structure, Heap for Priority queue, Heap]
---

<center>Basic code of Heap for Priority queue</center> 

## Abstract Data Type
```c
typedef struct _heap {
	PriorityComp comp;
	HData heapArr[HEAP_LEN];
	int numOfData;
}Heap;

void HeapInit(Heap* ph, PriorityComp pc);
- Initialize the heap and set Priority compare func.
int HIsEmpty(Heap* ph);
- if heap is empty return TRUE, else FALSE.
void HInsert(Heap* ph, HData data);
- Input data into heap.
HData HDelete(Heap* ph);
- Output data from heap. 
```


## Declaration
```c
#include"SimpleHeap.h"

void HeapInit(Heap* ph, PriorityComp pc) {
	ph->numOfData = 0;
	ph->comp = pc;
}
int HIsEmpty(Heap* ph) {
	if (ph->numOfData == 0)
		return TRUE;
	else
		return FALSE;
}
int GetParentIdx(int idx) {
	return idx / 2;
}
int GetLChildIdx(int idx) {
	return idx * 2;
}
int GetRChildIdx(int idx) {
	return idx * 2 +1;
}
int GetHiPriChildIDX(Heap* ph, int idx) {
	if (GetLChildIdx(idx) > ph->numOfData)
		return 0;
	else if (GetLChildIdx(idx) == ph->numOfData)
		return GetLChildIdx(idx);
	else
	{
		if(ph->comp(ph->heapArr[GetLChildIdx(idx)], ph->heapArr[GetRChildIdx(idx)]) < 0)
			return GetRChildIdx(idx);
		else
			return GetLChildIdx(idx);
	}
}
void HInsert(Heap* ph, HData data) {
	int idx = ph->numOfData + 1;
	
	while (idx != 1) {
		if (ph->comp(data,ph->heapArr[GetParentIdx(idx)]) > 0 ) {
			ph->heapArr[idx] = ph->heapArr[GetParentIdx(idx)];
			idx = GetParentIdx(idx);
		}
		else
			break;
	}
	ph->heapArr[idx] = data;
	(ph->numOfData)++;
}
HData HDelete(Heap* ph) {
	HData rdata = ph->heapArr[1];
	HData lastElem = ph->heapArr[ph->numOfData];

	int parentIdx = 1;
	int childIdx;

	while (childIdx = GetHiPriChildIDX(ph, parentIdx)) {
		if (ph->comp(lastElem,ph->heapArr[childIdx]) >= 0)
			break;
		
		ph->heapArr[parentIdx] = ph->heapArr[childIdx];
		parentIdx = childIdx;
	}
	ph->heapArr[parentIdx] = lastElem;
	(ph->numOfData)--;
	return rdata;

}
```

## Example Code
```c
#include<stdio.h>
#include"SimpleHeap.h"

int DataPriorityComp(char ch1, char ch2) {
	return ch2 - ch1;
}

int main() {
	Heap heap;
	HeapInit(&heap, DataPriorityComp);

	HInsert(&heap, 'A');
	HInsert(&heap, 'B');
	HInsert(&heap, 'C');
	printf("%c \n", HDelete(&heap));

	HInsert(&heap, 'A');
	HInsert(&heap, 'B');
	HInsert(&heap, 'C');
	printf("%c \n", HDelete(&heap));

	while (!HIsEmpty(&heap)) {
		printf("%c \n", HDelete(&heap));
	}
	return 0;
}
```
