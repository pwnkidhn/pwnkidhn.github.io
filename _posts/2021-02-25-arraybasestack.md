---
layout: post
title: "Array base Stack "
date: 2021-02-25
categories: DataStructure
tags: [Data Structure, Array base Stack, Stack]
---

<center>Basic code of Array base Stack</center> 

## Abstract Data Type
```c
typedef ArrayStack Stack;

void StackInit(Stack* pstack);
- Initialize the stack. should be called at first. 
int SIsEmpty(Stack* pstack);
- if stack is empty return TRUE, else FALSE.
void SPush(Stack* pstack, Data data);
- Input data into Stack.
Data SPop(Stack* pstack);
- Output data from Stack also remove the data.
Data SPeek(Stack* pstack);
- Output data from Stack. 

```


## Declaration
```c
#ifndef __AB_STACK_H__
#define __AB_STACK_H__

#define TRUE	1	
#define FALSE	0
#define STACK_LEN 100

typedef int Data;

typedef struct _arrayStack {
	
	Data stackArr[STACK_LEN];
	int topIndex;

}ArrayStack;

typedef ArrayStack Stack;

void StackInit(Stack* pstack);
int SIsEmpty(Stack* pstack);

void SPush(Stack* pstack, Data data);
Data SPop(Stack* pstack);
Data SPeek(Stack* pstack);

#endif
```

## Implementation
```c
#include"ArrayBaseStack.h"
#include<stdio.h>

void StackInit(Stack* pstack) {
	pstack->topIndex = -1;
}
int SIsEmpty(Stack* pstack) {
	if (pstack->topIndex == -1)
		return TRUE;
	else
		return FALSE;
}

void SPush(Stack* pstack, Data data) {
	(pstack->topIndex)++;
	pstack->stackArr[pstack->topIndex] = data;
}

Data SPop(Stack* pstack) {
	int rIdx;
	if (SIsEmpty(pstack)) {
		printf("Stack Memory Error!");
		exit(-1);
	}

	rIdx = pstack->topIndex;
	(pstack->topIndex)--;
	
	return pstack->stackArr[rIdx];
}

Data SPeek(Stack* pstack) {
	if (SIsEmpty(pstack)) {
		printf("Stack Memory Error!");
		exit(-1);
	}
	return pstack->stackArr[pstack->topIndex];
}

```

## Example Code
```c
#include<stdio.h>
#include"ArrayBaseStack.h"

int main() {
	Stack stack;

	StackInit(&stack);
	SPush(&stack, 1);
	SPush(&stack, 2);
	SPush(&stack, 3);
	SPush(&stack, 4);
	SPush(&stack, 5);

	while (!SIsEmpty(&stack)) {
		printf("%d ",SPop(&stack));
	}
	return 0;
}
```
