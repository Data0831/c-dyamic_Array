---
tags: Asserts
aliase: 
created: 2023-02-05 11:51
modified: 星期日 5日 二月 2023 11:51:45
---

# Asserts
***

```cpp
#pragma once
#include <iostream>

class Asserts {
public:
	static void test(bool comp) {
		try {
			if (comp == false)	throw "test error!";
		}
		catch (const char* msg) {
			std::cerr << msg << std::endl;
		}
	}
};
```
