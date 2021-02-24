---
layout: post
title: "Array List"
date: 2021-02-24
categories: DataStructure
tags: [Data Structure, Array List, List]
---

<center>Basic code of array List</center> 

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

```


## Declaration
```c
#ifndef __ARRAY_LIST_H__
#define __ARRAY_LIST_H__

#define TRUE	1	
#define FALSE	0

#define LIST_LEN	100

typedef int LData;

typedef struct __ArrayList {
	LData arr[LIST_LEN];
	int numOfData;
	int curPosition;
}ArrayList;

typedef ArrayList List;

void ListInit(List* plist);
void LInsert(List* plist, LData data);

int LFirst(List* plist, LData* pdata);
int LNext(List* plist, LData* pdata);

LData LRemove(List* plist);
int LCount(List* plist);

#endif
```

## Implementation
```c
#include"ArrayList.h"
#include<stdio.h>

void ListInit(List* plist) {
	(plist->numOfData) = 0;
	(plist->curPosition) = -1;
}

void LInsert(List* plist, LData data) {
	
	if (plist->numOfData >= LIST_LEN) {
		puts("Can't insert.");
		return;
	}

	plist->arr[plist->numOfData] = data;
	(plist->numOfData)++;
}

int LFirst(List* plist, LData* pdata) {
	if (plist->numOfData == 0) {
		return FALSE;
	}
	(plist->curPosition) = 0;
	*pdata = plist->arr[plist->curPosition];
	return TRUE;
}

int LNext(List* plist, LData* pdata) {
	if ((plist->curPosition) >= (plist->numOfData - 1)) {
		return FALSE;
	}
	(plist->curPosition)++;
	*pdata = plist->arr[plist->curPosition];
	return TRUE;
}

LData LRemove(List* plist) {

	int rpos = plist->curPosition;
	int num = plist->numOfData;
	int i = 0;
	LData rdata = plist->arr[rpos];
	
	for (i = rpos; i < num-1; i++) {
		plist->arr[i] = plist->arr[i + 1];
	}
	
	(plist->numOfData)--;
	(plist->curPosition)--;
	return rdata;
	   	  
}

int LCount(List* plist) {
	return plist->numOfData;
}
```

## Example Code
```c
#include<stdio.h>
#include"ArrayList.h"

int main() {
	List list;
	int data;
	
	ListInit(&list);
	LInsert(&list, 11);
	LInsert(&list, 22);
	LInsert(&list, 33);
	LInsert(&list, 44);
	LInsert(&list, 55);

	printf("numofData : %d\n", LCount(&list));

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

	printf("numofData : %d\n", LCount(&list));

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
