## 单例模式

**单例模式定义：**
> 确保一个类只有一个实例，并提供一个全局访问点来访问这个唯一实例。

**单例模式3个要点**  
* 这个类只能有一个实例
* 它必须自己创建这个实例
* 它必须自己向整个系统提供这个实例

## 单例模式结构  
![](https://github.com/lph6755065/Design-Patterns/blob/main/picture/20191020174208514.png)

## 单例模式代码及效果
单例模式代码：
```cpp
#ifndef __SINGLETON_H__
#define __SINGLETON_H__
 
#include <iostream>
#include <string.h>
using namespace std;
 
//抽象产品类AbstractBall
class Singleton
{
public:
	static Singleton* getInstance(){
		if (instance == NULL){
			printf("创建新的实例\n");
			instance = new Singleton();
		}
		return instance;
	}
private:
	Singleton(){}
 
	static Singleton* instance;
};
 
Singleton* Singleton::instance = NULL;
 
#endif //__SINGLETON_H__
```
