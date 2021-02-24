---
layout: post
title: "Doubly Linked List "
date: 2021-02-24
categories: DataStructure
tags: [Data Structure, Doubly Linked List, List]
---

<center>Basic code of Doubly Linked List</center> 

## Abstract Data Type
```c
typedef int Data;

void ListInit(List* plist);
- Initialize the list. should be called at first 
    
void LInsert(List* plist, LData data);
- Insert data into List.
    
int LFirst(List* plist, LData* pdata);
- The First data is saved on *pdata. 
- if there is no data return FALSE else TRUE.
    
int LNext(List* plist, LData* pdata);
- The Next data is saved on *pdata (using with while loop)
- if there is no data return FALSE else TRUE.

int LPrevious(List* plist, LData* pdata);
- The Previous data is saved on *pdata (using with while loop)
- if there is no data return FALSE else TRUE.

Data LRemove(List* plist);
- Remove the data currently in the list and return the data.
    
int LCount(List* plist);
- return number of list data.

```


## Declaration
```c
#ifndef __DB_LINKED_LIST_H__
#define __DB_LINKED_LIST_H__

#define TRUE	1
#define FALSE	0

typedef int Data;

typedef struct _node {
	Data data;
	struct _node* next;
	struct _node* prev;
}Node;

typedef struct _DBLinkedList {
	Node* cur;
	Node* head;
	int numOfData;
}DBLinkedList;

typedef DBLinkedList List;

void ListInit(List* plist);
void LInsert(List* plist, Data data);

int LFirst(List* plist, Data* pdata);
int LNext(List* plist, Data* pdata);
int LPrevious(List* plist, Data* pdata);

Data LRemove(List* plist);
int LCount(List* plist);

#endif
```

## Implementation
```c
#include<stdlib.h>
#include"DBLinkedlist.h"

void ListInit(List* plist) {
	plist->numOfData = 0;
	plist->cur = NULL;
	plist->head = NULL;
}
void LInsert(List* plist, Data data) {
	Node* newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	newNode->next = plist->head;
	if (plist->head != NULL) {
		plist->head->prev = newNode;
	}
	newNode->prev = NULL;
	plist->head = newNode;

	(plist->numOfData)++;
}

int LFirst(List* plist, Data* pdata) {
	if (plist->head == NULL)
		return FALSE;
	plist->cur = plist->head;
	*pdata = plist->cur->data;
	return TRUE;
}
int LNext(List* plist, Data* pdata) {
	if (plist->cur->next == NULL)
		return FALSE;
	plist->cur = plist->cur->next;
	*pdata = plist->cur->data;
	return TRUE;
}
int LPrevious(List* plist, Data* pdata) {
	if (plist->cur->prev == NULL)
		return FALSE;
	plist->cur = plist->cur->prev;
	*pdata = plist->cur->data;
	return TRUE;
}

Data LRemove(List* plist) {
	Node* rpos = plist->cur;
	Data rdata = rpos->data;
	
	if (rpos == plist->head) {
		if (plist->head->next == NULL)
			plist->head = NULL;
		else {
			plist->head = rpos->next;
			plist->cur = plist->head;
			plist->cur->prev = NULL;
		}	
	}
	else {
		if (plist->cur->next != NULL) {
			plist->cur->next->prev = plist->cur->prev;
		}
		plist->cur->prev->next = plist->cur->next;
		plist->cur = plist->cur->prev;
	}
	
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
#include"DBLinkedlist.h"

int main() {

	List list;
	int data;
	ListInit(&list);

	LInsert(&list, 1);
	LInsert(&list, 2);
	LInsert(&list, 3);
	LInsert(&list, 4);
	LInsert(&list, 5);
	LInsert(&list, 6);
	LInsert(&list, 7);
	LInsert(&list, 8);

	if (LFirst(&list, &data)) {
		printf("%d ", data);

		while (LNext(&list, &data)) {
			printf("%d ", data);
		}
		while (LPrevious(&list, &data)) {
			printf("%d ", data);
		}
	}
	printf("\n\n");

	if (LFirst(&list, &data)) {
		if (data % 2 != 0)
			LRemove(&list);

		while (LNext(&list, &data)) {
			if (data % 2 != 0)
				LRemove(&list);
		}
	}
	if (LFirst(&list, &data)) {
		printf("%d ", data);

		while (LNext(&list, &data)) {
			printf("%d ", data);
		}
		while (LPrevious(&list, &data)) {
			printf("%d ", data);
		}
	}
	return 0;
}
```

