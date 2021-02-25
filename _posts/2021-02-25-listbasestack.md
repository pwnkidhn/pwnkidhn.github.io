---
layout: post
title: "List base Stack "
date: 2021-02-25
categories: DataStructure
tags: [Data Structure, List base Stack, Stack]
---

<center>Basic code of List base Stack</center> 

## Abstract Data Type
```c
typedef ListStack Stack;

void StackInit(Stack* pstack);
- Initialize the stack. should be called at first 
int SIsEmpty(Stack* pstack);
- if stack is empty return TRUE, else FALSE
void SPush(Stack* pstack, Data data);
- Input data into Stack
Data SPop(Stack* pstack);
- Output data from Stack also remove the data
Data SPeek(Stack* pstack);
- Output data from Stack 

```


## Declaration
```c
#include"ListbaseStack.h"
#include<stdlib.h>
#include<stdio.h>

void StackInit(Stack* pstack) {
	pstack->head = NULL;
}

int SIsEmpty(Stack* pstack) {
	if (pstack->head == NULL)
		return TRUE;
	else
		return FALSE;
}

void SPush(Stack* pstack, Data data) {
	Node* newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;
	newNode->next = pstack->head;
	pstack->head = newNode;
}

Data SPop(Stack* pstack) {
	
	if (SIsEmpty(pstack)) {
		printf("Stack Memory Error");
		exit(-1);
	}

	Node* rpos = pstack->head;
	Data rdata = rpos->data;

	pstack->head = pstack->head->next;
	
	free(rpos);
	return rdata;
}
Data SPeek(Stack* pstack) {
	if (SIsEmpty(pstack)) {
		printf("Stack Memory Error");
		exit(-1);
	}
	return pstack->head->data;
}
```

## Example Code
```c
#include<stdio.h>
#include"ListbaseStack.h"

int main() {
	Stack stack;

	StackInit(&stack);
	SPush(&stack, 1);
	SPush(&stack, 2);
	SPush(&stack, 3);
	SPush(&stack, 4);
	SPush(&stack, 5);

	while (!SIsEmpty(&stack)) {
		printf("%d ", SPop(&stack));
	}
	return 0;
}
```
