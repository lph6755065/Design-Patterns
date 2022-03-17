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
客户端验证
```cpp
int main()
{
	Singleton *s1 = Singleton::getInstance();
	Singleton *s2 = Singleton::getInstance();
 
	system("pause");
	return 0;
}
``` 
结果只出现一个实例

可以看出，构造函数是私有的，即单例模式对象只能在类内部实例化（满足单例模式第二个要点）。同时，实例对象Instance是静态static的，也就是全局的，假设客户端实例化了两个Singleton,但instance只有一个（静态共享的，满足第一个要点）。第三个要点是main里面调用系统提供的实例。

## 多线程环境测试单例模式 
```cpp
/*非线程安全 单例模式*/
#include <process.h>
#include <Windows.h>
#define THREAD_NUM 5
 
unsigned int __stdcall CallSingleton(void *pPM)
{
	Singleton *s = Singleton::getInstance();
	int nThreadNum = *(int *)pPM; 
	Sleep(50);
	printf("线程编号为%d\n", nThreadNum);
	return 0;
}
 
int main()
{
	HANDLE  handle[THREAD_NUM];
 
	//线程编号
	int threadNum = 0;
	while (threadNum < THREAD_NUM)
	{
		handle[threadNum] = (HANDLE)_beginthreadex(NULL, 0, CallSingleton, &threadNum, 0, NULL);
		//等子线程接收到参数时主线程可能改变了这个i的值
		threadNum++;
	}
	//保证子线程已全部运行结束
	WaitForMultipleObjects(THREAD_NUM, handle, TRUE, INFINITE);
 
	system("pause");
	return 0;
}
```
![](https://github.com/lph6755065/Design-Patterns/blob/main/picture/20191020174208514.png)
## 线程安全的单例模式代码
>可以用线程同步与互斥的方式  

```cpp
#ifndef __SINGLETON_H__
#define __SINGLETON_H__
 
#include <iostream>
#include <string.h>
#include <mutex>
using namespace std;
 
class Singleton
{
public:
	static Singleton* getInstance(){
		if (instance == NULL){
			m_mutex.lock();
			if (instance == NULL){
				printf("创建新的实例\n");
				instance = new Singleton();
			}
			m_mutex.unlock();
		}
		return instance;
	}
private:
	Singleton(){}
 
	static Singleton* instance;
	static std::mutex m_mutex;
};
 
Singleton* Singleton::instance = NULL;
std::mutex Singleton::m_mutex;
 
#endif //__SINGLETON_H__
```  
在多线程环境测试结果，功能正常
![](https://github.com/lph6755065/Design-Patterns/blob/main/picture/20191020183942253.png)

## 单例模式总结
**优点：**  
* 单例模式提供了严格的对唯一实例的创建和访问。
* 单例模式的实现可以节省系统资源

**缺点** 
* 如果某个实例负责多重职责但又必须实例唯一，那单例职责过多，这违背了单一职责原则
* 多线程下需要考虑线程安全机制
* 单例模式没有抽象层，不方便扩展

**适用环境：**
* 系统只需要一个实例对象
* 某个实例只允许有一个访问接口

**应用实例**
* windows任务管理器

## 单例模式面试考点


