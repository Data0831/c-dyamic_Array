---
tags: C++ List.h
aliase: 
created: 2023-02-05 10:03
modified: 星期日 5日 二月 2023 10:04:06
---

# C++ List.h
***
>作為Array 的公共街口

```cpp
#pragma once

template<typename T>
class List {
protected:
	int size = 0, capacity;
	void rangeCheck(int idx) const;
	void rangeCheckForAdd(int idx) const;
public:
	static const int ELEMENT_NOT_FOUND = -1;
	int getSize() const;
	void add(T ele);
	virtual void add(int idx, T ele) = 0;
	virtual T remove(int idx) = 0;
	virtual T get(int idx) const = 0;
	virtual T set(int idx, T ele) = 0;
	virtual int indexOf(T ele) const = 0;
	bool contains(T ele) const;
	virtual void clear() = 0;
};

template<typename T>
void List<T>::rangeCheck(int idx) const {
	try {
		if (idx < 0 || idx > List<T>::size - 1) throw "Index Error: range";
	}
	catch (const char* msg) {
		std::cerr << msg << std::endl;
		std::cerr << "size: " << this->size << " index: " << idx << std::endl;
		exit(1);
	}

}

template<typename T>
void List<T>::rangeCheckForAdd(int idx) const {
	try {
		if (idx < 0 || idx > List<T>::size) throw "Index Error: Add";
	}
	catch (const char* msg) {
		std::cerr << msg << std::endl;
		std::cerr << "size: " << this->size << " index: " << idx << std::endl;
		exit(1);
	}

}

template<typename T>
int List<T>::getSize() const{
	return size;
}

template<typename T>
void List<T>::add(T ele) {
	add(size, ele);
}

template<typename T>
bool List<T>::contains(T ele) const{
	return indexOf(ele) != ELEMENT_NOT_FOUND;
}
```
