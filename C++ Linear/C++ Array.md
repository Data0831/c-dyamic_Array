---
tags: C++ Array
aliase: 
created: 2023-02-05 10:03
modified: 星期日 5日 二月 2023 10:03:46
---

# C++ Array
***
>完整版 trim ensureCapacity exception trim ccaacity

```cpp
#pragma once
#include <iostream>
#include "List.h"

template<typename T>
class Array :public List<T> {
	T* arr;
	int first = 0;
	int index(int idx) const;
	void ensureCapacity(int size);
	void trim(int size);
public:
	Array();
	Array(int size);
	~Array();
	static const int DEFAULT_CAPCITY = 10;
	void add(T ele);
	void add(int idx, T ele)override;
	T remove(int idx) override;
	T get(int idx) const override;
	T set(int idx, T ele) override;
	int indexOf(T ele) const override;
	void clear() override;

	template<typename U>
	friend std::ostream& operator << (std::ostream& out, Array<U>& arr);
};

template<typename T>
int Array<T>::index(int idx) const {
	idx += first;
	if (idx >= this->capacity) return idx - this->capacity;
	else if (idx < 0) return idx + this->capacity;
	else return idx;
}

template<typename T>
void Array<T>::ensureCapacity(int size) {
	if (size != List<T>::capacity) return;
	int newCapacity = (List<T>::capacity >> 1) + List<T>::capacity;
	T* newArr = new T[newCapacity];
	for (int i = 0; i < this->size; i++) {
		newArr[i] = arr[index(i)];
	}

	std::cout << List<T>::capacity << " -> " << newCapacity << std::endl;
	delete[] arr;
	arr = newArr;
	List<T>::capacity = newCapacity;
	first = 0;
}

template<typename T>
void Array<T>::trim(int size) {
	if (size >= (List<T>::capacity >> 2) || size < (DEFAULT_CAPCITY >> 1)) return;
	int newCapacity = (List<T>::capacity >> 1);
	T* newArr = new T[newCapacity];
	for (int i = 0; i < this->size; i++) {
		newArr[i] = arr[index(i)];
	}

	std::cout << List<T>::capacity << " -> " << newCapacity << std::endl;
	delete[] arr;
	arr = newArr;
	List<T>::capacity = newCapacity;
	first = 0;
}

template<typename T>
Array<T>::Array() {
	arr = new T[DEFAULT_CAPCITY];
	List<T>::capacity = DEFAULT_CAPCITY;
}

template<typename T>
Array<T>::Array(int size) {
	if (size <= DEFAULT_CAPCITY) Array();
	else {
		arr = new T[size];
		List<T>::capacity = size;
	}
}

template<typename T>
Array<T>::~Array() {
	delete[] arr;
}

template<typename T>
void Array<T>::add(T ele) {
	List<T>::add(ele);
}


template<typename T>
void Array<T>::add(int idx, T ele) {
	this->rangeCheckForAdd(idx);
	ensureCapacity(List<T>::size + 1);

	if (idx < (this->size >> 1)) {
		for (int i = 0; i < idx; i++) {
			arr[index(i - 1)] = arr[index(i)];
		}
		first = index(-1);
	}
	else {
		for (int i = List<T>::size; i > idx; i--) {
			arr[index(i)] = arr[index(i - 1)];
		}
	}

	arr[index(idx)] = ele;
	List<T>::size++;
}

template<typename T>
T Array<T>::remove(int idx) {
	this->rangeCheck(idx);
	T old = arr[index(idx)];

	if (idx < (this->size >> 1)) {
		for (int i = idx; i > 0; i--) {
			arr[index(i)] = arr[index(i - 1)];
		}

		arr[index(0)] = NULL;
		first = index(1);
		
	}
	else {
		for (int i = idx; i < List<T>::size - 1; i++) {
			arr[index(i)] = arr[index(i + 1)];
		}

		arr[index(this->size-1)] = NULL;
	}

	--List<T>::size;
	trim(List<T>::size);
	return old;
}

template<typename T>
T Array<T>::get(int idx) const {
	this->rangeCheck(idx);
	return arr[index(idx)];
}

template<typename T>
T Array<T>::set(int idx, T ele) {
	this->rangeCheck(idx);
	T old = arr[index(idx)];
	arr[index(idx)] = ele;
	return old;
}

template<typename T>
int Array<T>::indexOf(T ele) const {
	for (int i = 0; i < List<T>::size; i++) {
		if (arr[index(i)] == ele)	return i;
	}

	return List<T>::ELEMENT_NOT_FOUND;
}

template<typename T>
void Array<T>::clear() {
	for (int i = 0; i < List<T>::size; i++) {
		arr[index(i)] = NULL;
	}
	List<T>::size = 0;
	trim(List<T>::size + 1);
}

template<typename T>
std::ostream& operator << (std::ostream& out, Array<T>& arr) {
	out << "[";
	for (int i = 0; i < arr.size; i++) {
		if (i != 0)	out << ", ";
		out << arr.arr[arr.index(i)];
	}
	out << "]\n\n";
	return out;
}
```
