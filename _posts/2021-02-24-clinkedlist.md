---
layout: post
title: "Circular Linked List "
date: 2021-02-24
categories: DataStructure
tags: [Data Structure, Circular Linked List, List]
---

<center>Basic code of Circular Linked List</center> 

## Abstract Data Type
```c
typedef int Data;

void ListInit(List* plist);
- Initialize the list. should be called at first 
    
void LInsert(List* plist, LData data);
- Insert data into List at Tail.

void LInsertFront(List* plist, LData data);
- Insert data into List at Head.

int LFirst(List* plist, LData* pdata);
- The First data is saved on *pdata. 
- if there is no data return FALSE else TRUE.
    
int LNext(List* plist, LData* pdata);
- The Next data is saved on *pdata (using with while loop)
- if there is no data return FALSE else TRUE.
    
Data LRemove(List* plist);
- Remove the data currently in the list and return the data.
    
int LCount(List* plist);
- return number of list data.

```


## Declaration
```c
#ifndef __C_LINKED_LIST_H__
#define	__C_LINKED_LIST_H__

#define TRUE	1	
#define FALSE	0

typedef int Data;

typedef struct _node {
	Data data;
	struct _node* next;
}Node;

typedef struct _CLL {
	Node* cur;
	Node* tail;
	Node* before;
	int numOfData;
}CList;

typedef CList List;

void ListInit(List* plist);
void LInsert(List* plist, Data data);		// Insert at tail
void LInsertFront(List* plist, Data data);	// Insert at head

int LFirst(List* plist, Data* pdata);
int LNext(List* plist, Data* pdata);
Data LRemove(List* plist);
int LCount(List* plist);

#endif
```

## Implementation
```c
#include<stdlib.h>
#include"CLinkedList.h"

void ListInit(List* plist) {
	plist->numOfData = 0;
	plist->tail = NULL;
	plist->before = NULL;
	plist->cur = NULL;
}

void LInsert(List* plist, Data data) {
	Node* newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	if (plist->tail == NULL) {
		plist->tail = newNode;
		newNode->next = newNode;
	}
	else {
		newNode->next = plist->tail->next;
		plist->tail->next = newNode;
		plist->tail = newNode;	
	}
	(plist->numOfData)++;
}

void LInsertFront(List* plist, Data data) {
	Node* newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	if (plist->tail == NULL) {
		plist->tail = newNode;
		newNode->next = newNode;
	}
	else {
		newNode->next = plist->tail->next;
		plist->tail->next = newNode;
	}
	(plist->numOfData)++;
}

int LFirst(List* plist, Data* pdata) {
	if (plist->tail == NULL)
		return FALSE;

	plist->before = plist->tail;
	plist->cur = plist->tail->next;

	*pdata = plist->cur->data;
	return TRUE;
}

int LNext(List* plist, Data* pdata) {
	if (plist->tail == NULL)
		return FALSE;

	plist->before = plist->cur;
	plist->cur = plist->cur->next;

	*pdata = plist->cur->data;
	return TRUE;
}

Data LRemove(List* plist) {
	Node* rpos = plist->cur;
	Data rdata = rpos->data;

	if (rpos == plist->tail) {
		if (plist->tail->next == plist->tail) {
			plist->tail = NULL;
		}
		else {
			plist->tail = plist->before;
		}
	}

	plist->before->next = plist->cur->next;
	plist->cur = plist->before;
	
	free(rpos);
	(plist->numOfData)--;
	return rdata;
}

int LCount(List* plist) {
	return plist->numOfData;
}
```

## Example Code
```c
#include<stdio.h>
#include<stdlib.h>
#include"CLinkedList.h"

int main() {

	List list;
	int data, i, nodeNum;
	ListInit(&list);

	LInsert(&list, 3);
	LInsert(&list, 4);
	LInsert(&list, 5);
	LInsertFront(&list, 2);
	LInsertFront(&list, 1);

	if (LFirst(&list, &data)) {
		printf("%d ", data);
		
		for (i = 0; i < LCount(&list) * 3 - 1; i++) {
			if (LNext(&list, &data))
				printf("%d ", data);
		}
	}
	printf("\n\n");
	nodeNum = LCount(&list);
	if (nodeNum != 0) {
		LFirst(&list, &data);
		if (data % 2 == 0)
			LRemove(&list);

		for (i = 0; i < nodeNum - 1; i++) {
			LNext(&list, &data);
			if (data % 2 == 0)
				LRemove(&list);
		}
	}
	if (LFirst(&list, &data)) {
		printf("%d ", data);

		for (i = 0; i < LCount(&list)- 1; i++) {
			if (LNext(&list, &data))
				printf("%d ", data);
		}
	}
	return 0;
}
```
