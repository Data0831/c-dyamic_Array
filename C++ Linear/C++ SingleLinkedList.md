---
tags: C++ 
aliase: 
created: 2023-02-07 13:28
modified: 星期三 8日 二月 2023 17:24:22
---

# C++ SingleLinkedList
***
>normal version

```cpp
#pragma once
#include<iostream>
#include "List.h"

template<typename T>
class SingleLinkedList : public List<T> {
	static class Node {
	public:
		T ele;
		Node* next;
		Node() {
		}
		Node(T ele, Node* next) : ele(ele), next(next) {
		}
	};

	Node* first = nullptr;
	Node* node(int idx) const;
public:
	void add(T ele);
	void add(int idx, T ele) override;
	T remove(int idx) override;
	T get(int idx) const override;
	T set(int idx, T ele) override;
	int indexOf(T ele) const override;
	void clear() override;

	template<typename U>
	friend std::ostream& operator << (std::ostream& out, SingleLinkedList<U>& list);
};

template<typename T>
typename SingleLinkedList<T>::Node* SingleLinkedList<T>::node(int idx) const {
	this->rangeCheck(idx);
	Node* node = first;
	for (int i = 0; i < idx; i++) {
		node = node->next;
	}

	return node;
}

template<typename T>
void SingleLinkedList<T>::add(T ele) {
	List<T>::add(ele);
}


template<typename T>
void SingleLinkedList<T>::add(int idx, T ele) {
	this->rangeCheckForAdd(idx);

	if (idx == 0) {
		first = new Node(ele, first);
	}
	else {
		Node* prev = node(idx - 1);
		prev->next = new Node(ele, prev->next);
	}

	List<T>::size++;
}

template<typename T>
T SingleLinkedList<T>::remove(int idx) {
	Node* now = node(idx);
	T old = now->ele;

	if (idx == 0) {
		first = now->next;
	}
	else {
		Node* prev = node(idx - 1);
		prev->next = now->next;
	}

	delete now;
	List<T>::size--;
	return old;
}

template<typename T>
T SingleLinkedList<T>::get(int idx) const {
	return node(idx)->ele;
}

template<typename T>
T SingleLinkedList<T>::set(int idx, T ele) {
	Node* now = node(idx);
	T old = now->ele;
	now->ele = ele;
	return old;
}

template<typename T>
int SingleLinkedList<T>::indexOf(T ele) const {
	Node* now = first;

	for (int i = 0; now != nullptr; i++) {
		if (now->ele == ele) {
			return i;
		}
		now = now->next;
	}

	return List<T>::ELEMENT_NOT_FOUND;
}

template<typename T>
void SingleLinkedList<T>::clear() {
	Node* prev = first, * next;

	for (int i = 0; first != nullptr; i++) {
		next = prev->next;
		delete prev;
		prev = next;
	}

	List<T>::size = 0;
}

template<typename T>
std::ostream& operator << (std::ostream& out, SingleLinkedList<T>& list) {
	typename SingleLinkedList<T>::Node* now = list.first;

	out << "[";
	for (int i = 0; now != nullptr; i++) {
		if (i != 0)	out << ", ";
		out << now->ele;
		now = now->next;
	}
	out << "]\n\n";
	return out;
}
```
