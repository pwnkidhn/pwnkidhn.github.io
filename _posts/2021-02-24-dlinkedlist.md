---
layout: post
title: "Dummy Linked List "
date: 2021-02-24
categories: DataStructure
tags: [Data Structure, Dummy Linked List, List]
---

<center>Basic code of Dummy Linked List</center> 

## Abstract Data Type
```c
typedef int LData;

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
    
LData LRemove(List* plist);
- Remove the data currently in the list and return the data.
    
int LCount(List* plist);
- return number of list data.

void SetSortRule(List* plist, int (*comp)(LData d1, LData d2));
- set the rule of sort using to insert data into List.

```


## Declaration
```c
#ifndef __D_LINKED_LIST_H__
#define __D_LINKED_LIST_H__
#define TRUE	1
#define FALSE	0

typedef int LData;

typedef struct _node {
	LData data;
	struct _node* next;
}Node;

typedef struct _linkedlist {
	Node* cur;
	Node* head;
	Node* before;
	int numOfData;
	int (*comp)(LData d1, LData d2);
}LinkedList;

typedef LinkedList List;

void ListInit(List* plist);
void LInsert(List* plist, LData data);

int LFirst(List* plist, LData* pdata);
int LNext(List* plist, LData* pdata);

LData LRemove(List* plist);
int LCount(List* plist);

void SetSortRule(List* plist, int(*comp)(LData d1, LData d2));

#endif
```

## Implementation
```c
#include<stdio.h>
#include"DLinkedList.h"

void ListInit(List* plist) {
	plist->head = (Node*)malloc(sizeof(Node));
	plist->head->next = NULL;
	plist->comp = NULL;
	plist->numOfData = 0;
}

void FInsert(List* plist, LData data) {
	Node* newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	newNode->next = plist->head->next;
	plist->head->next = newNode;
	(plist->numOfData)++;
}

void SInsert(List* plist, LData data) {
	Node* newNode = (Node*)malloc(sizeof(Node));
	Node* pred = plist->head;

	newNode->data = data;

	//the function comp will return 0, when d1 < d2   (head - d1 - d2)
	while (pred->next != NULL && plist->comp(data, pred->next->data) != 0) {
		pred = pred->next;
	}

	newNode->next = pred->next;
	pred->next = newNode;

	(plist->numOfData)++;
}

void LInsert(List* plist, LData data) {
	if (plist->comp == NULL)
		FInsert(plist,data);
	else
		SInsert(plist,data);
}

int LFirst(List* plist, LData* pdata) {
	if (plist->head->next == NULL)
		return FALSE;

	plist->before = plist->head;
	plist->cur = plist->head->next;
	
	*pdata = plist->cur->data;
	return TRUE;
}

int LNext(List* plist, LData* pdata) {
	if (plist->cur->next == NULL)
		return FALSE;

	plist->before = plist->cur;
	plist->cur = plist->cur->next;

	*pdata = plist->cur->data;
	return TRUE;
}

LData LRemove(List* plist) {
	Node* rpos = plist->cur;
	LData rdata = rpos->data;

	plist->before->next = plist->cur->next;
	plist->cur = plist->before;

	free(rpos);
	(plist->numOfData)--;
	return rdata;
}

int LCount(List* plist) {
	return plist->numOfData;
}

void SetSortRule(List* plist, int(*comp)(LData d1, LData d2)) {
	plist->comp = comp;
}
```

## Example Code
```c
#include<stdio.h>
#include"DLinkedList.h"

int WhoIsPrecede(int d1, int d2) {
	if (d1 < d2)
		return 0;		
	else
		return 1;
}

int main() {
	List list;
	int data;
	
	ListInit(&list);
	SetSortRule(&list, WhoIsPrecede);

	LInsert(&list, 11);
	LInsert(&list, 11);
	LInsert(&list, 22);
	LInsert(&list, 22);
	LInsert(&list, 33);

	printf("num of data : %d\n", LCount(&list));

	if (LFirst(&list, &data)) {
		printf("%d ", data);

		while (LNext(&list, &data)) {
			printf("%d ", data);
		}
	}
	printf("\n\n");

	if (LFirst(&list, &data)) {
		if (data == 22)
			LRemove(&list);

		while (LNext(&list, &data)) {
			if (data == 22)
				LRemove(&list);
		}
	}
	printf("num of data : %d\n", LCount(&list));

	if (LFirst(&list, &data)) {
		printf("%d ", data);

		while (LNext(&list, &data)) {
			printf("%d ", data);
		}
	}
	printf("\n\n");

	return 0;
}
```
