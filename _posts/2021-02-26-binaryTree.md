---
layout: post
title: "Binary Tree"
date: 2021-02-26
categories: DataStructure
tags: [Data Structure, Binary Tree, Tree]
---

<center>Basic code of Binary Tree</center> 

## Abstract Data Type
```c
BTreeNode* MakeBTreeNode(void);
- Make binary tree. Initialize the tree.
BTData GetData(BTreeNode* bt);
- Output data from the tree.
void SetData(BTreeNode* bt, BTData data);
- Input data into the tree.
BTreeNode* GetLeftSubTree(BTreeNode* bt);
- Output Leftside subTree from the tree.
BTreeNode* GetRightSubTree(BTreeNode* bt);
- Output Rightside subTree from the tree.
void MakeLeftSubTree(BTreeNode* main, BTreeNode* sub);
- Input subTree into the main tree's leftside.
void MakeRightSubTree(BTreeNode* main, BTreeNode* sub);
- Input subTree into the main tree's rightside.

typedef void (*visitFunc)(BTData data);

void InorderTraverse(BTreeNode(*bt), visitFunc action);
- Search tree (left -> root -> right)
void PreorderTraverse(BTreeNode(*bt), visitFunc action);
- Search tree (root -> left -> right)
void PostorderTraverse(BTreeNode(*bt), visitFunc action);
- Search tree (left -> right -> root)
void DeleteTree(BTreeNode* bt);
- Remove tree.
```


## Declaration
```c
#include"BinaryTree.h"
#include<stdlib.h>

BTreeNode* MakeBTreeNode(void) {
	BTreeNode* btNode = (BTreeNode*)malloc(sizeof(BTreeNode));
	btNode->left = NULL;
	btNode->right = NULL;
	return btNode;
}
BTData GetData(BTreeNode* bt) {
	return bt->data;
}
void SetData(BTreeNode* bt, BTData data) {
	bt->data = data;
}
BTreeNode* GetLeftSubTree(BTreeNode* bt) {
	return bt->left;
}
BTreeNode* GetRightSubTree(BTreeNode* bt) {
	return bt->right;
}
void MakeLeftSubTree(BTreeNode* main, BTreeNode* sub) {
	if (main->left != NULL)
		free(main->left);
	main->left = sub;
}
void MakeRightSubTree(BTreeNode* main, BTreeNode* sub) {
	if (main->right != NULL)
		free(main->right);
	main->right = sub;
}
void InorderTraverse(BTreeNode(*bt), visitFunc action) {
	if (bt == NULL)
		return;

	InorderTraverse(bt->left, action);
	action(bt->data);
	InorderTraverse(bt->right, action);
}
void PreorderTraverse(BTreeNode(*bt), visitFunc action) {
	if (bt == NULL)
		return;

	action(bt->data);
	PreorderTraverse(bt->left, action);
	PreorderTraverse(bt->right, action);
}
void PostorderTraverse(BTreeNode(*bt), visitFunc action) {
	if (bt == NULL)
		return;

	PostorderTraverse(bt->left, action);
	PostorderTraverse(bt->right, action);
	action(bt->data);
}
void DeleteTree(BTreeNode* bt) {
	if (bt == NULL)
		return;
		
	DeleteTree(bt->left);
	DeleteTree(bt->right);
	printf("delete %d\n", bt->data);
	free(bt);
}
```

## Example Code
```c
#include<stdio.h>
#include"BinaryTree.h"

void ShowData(BTData data);

int main() {
	BTreeNode* bt1 = MakeBTreeNode();
	BTreeNode* bt2 = MakeBTreeNode();
	BTreeNode* bt3 = MakeBTreeNode();
	BTreeNode* bt4 = MakeBTreeNode();
	BTreeNode* bt5 = MakeBTreeNode();
	BTreeNode* bt6 = MakeBTreeNode();

	SetData(bt1, 1);
	SetData(bt2, 2);
	SetData(bt3, 3);
	SetData(bt4, 4);
	SetData(bt5, 5);
	SetData(bt6, 6);

	MakeLeftSubTree(bt1, bt2);
	MakeRightSubTree(bt1, bt3);
	MakeLeftSubTree(bt2, bt4);
	MakeRightSubTree(bt2, bt5);
	MakeRightSubTree(bt3, bt6);

	PreorderTraverse(bt1, ShowData);
	printf("\n");
	InorderTraverse(bt1, ShowData);
	printf("\n");
	PostorderTraverse(bt1, ShowData);
	DeleteTree(bt1);

	return 0;
}

void ShowData(BTData data) {
	printf("%d ", data);
}
```
