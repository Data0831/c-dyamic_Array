---
tags: C++ Array2
aliase: 
created: 2023-02-05 18:11
modified: 星期日 5日 二月 2023 18:11:42
---

# C++ Array2
***
>可以放入nullptr了
>將T* 改為 T**
>陣列現在裡面存的是記憶體位址
>需手動對每個元素做內存釋放

>Java 用的是物件指標，所以默認都為2層指標
```cpp
#pragma once
#include <iostream>
#include "List.h"

template<typename T>
class Array2 :public List<T> {
	T** arr;
	int first = 0;
	int index(int idx) const;
	void ensureCapacity(int size);
	void trim(int size);
public:
	Array2();
	Array2(int size);
	~Array2();
	static const int DEFAULT_CAPCITY = 10;
	void add(T ele);
	void add(void* ele);
	void add(int idx, void* ele);
	void add(int idx, T ele)override;
	T remove(int idx) override;
	T get(int idx) const override;
	T set(int idx, T ele) override;
	int indexOf(T ele) const override;
	void clear() override;

	template<typename U>
	friend std::ostream& operator << (std::ostream& out, Array2<U>& arr);
};

template<typename T>
int Array2<T>::index(int idx) const {
	idx += first;
	if (idx >= this->capacity) return idx - this->capacity;
	else if (idx < 0) return idx + this->capacity;
	else return idx;
}

template<typename T>
void Array2<T>::ensureCapacity(int size) {
	if (size != List<T>::capacity) return;
	int newCapacity = (List<T>::capacity >> 1) + List<T>::capacity;
	T** newArr = new T*[newCapacity];
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
void Array2<T>::trim(int size) {
	if (size >= (List<T>::capacity >> 2) || size < (DEFAULT_CAPCITY >> 1)) return;
	int newCapacity = (List<T>::capacity >> 1);
	T** newArr = new T* [newCapacity];
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
Array2<T>::Array2() {
	arr = new T * [DEFAULT_CAPCITY];
	List<T>::capacity = DEFAULT_CAPCITY;
}

template<typename T>
Array2<T>::Array2(int size) {
	if (size <= DEFAULT_CAPCITY) Array2();
	else {
		arr = new T * [size];
		List<T>::capacity = size;
	}
}

template<typename T>
Array2<T>::~Array2() {
	delete[] arr;
}

template<typename T>
void Array2<T>::add(T ele) {
	List<T>::add(ele);
}

template<typename T>
void Array2<T>::add(void* ele){
	add(this->size, ele);
}

template<typename T>
void Array2<T>::add(int idx, T ele) {
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

	arr[index(idx)] = new T(ele);
	List<T>::size++;
}

template<typename T>
void Array2<T>::add(int idx, void* ele) {
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

	arr[index(idx)] = nullptr;
	List<T>::size++;
}

template<typename T>
T Array2<T>::remove(int idx) {
	this->rangeCheck(idx);
	T old = *arr[index(idx)];
	delete arr[index(idx)];

	if (idx < (this->size >> 1)) {
		for (int i = idx; i > 0; i--) {
			arr[index(i)] = arr[index(i - 1)];
		}

		arr[index(0)] = nullptr;
		first = index(1);

	}
	else {
		for (int i = idx; i < List<T>::size - 1; i++) {
			arr[index(i)] = arr[index(i + 1)];
		}

		arr[index(this->size - 1)] = nullptr;
	}

	--List<T>::size;
	trim(List<T>::size);
	return old;
}

template<typename T>
T Array2<T>::get(int idx) const {
	this->rangeCheck(idx);
	return *arr[index(idx)];
}

template<typename T>
T Array2<T>::set(int idx, T ele) {
	this->rangeCheck(idx);
	T old = *arr[index(idx)];
	*arr[index(idx)] = ele;
	return old;
}

template<typename T>
int Array2<T>::indexOf(T ele) const {
	for (int i = 0; i < List<T>::size; i++) {
		if (*arr[index(i)] == ele)	return i;
	}

	return List<T>::ELEMENT_NOT_FOUND;
}

template<typename T>
void Array2<T>::clear() {
	for (int i = 0; i < List<T>::size; i++) {
		arr[index(i)] = NULL;
	}
	List<T>::size = 0;
	trim(List<T>::size + 1);
}

template<typename T>
std::ostream& operator << (std::ostream& out, Array2<T>& arr) {
	out << "[";
	for (int i = 0; i < arr.size; i++) {
		if (i != 0)	out << ", ";
		if (arr.arr[arr.index(i)] == nullptr)	out << "null";
		else out << *arr.arr[arr.index(i)];
	}
	out << "]\n\n";
	return out;
}
```
