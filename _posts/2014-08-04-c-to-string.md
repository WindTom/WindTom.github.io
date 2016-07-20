---
layout: post
title: "C++通过ostringstream实现任意类型转string"
description: ""
category: C++
tags: [C++,ostringstream]
---

{% highlight ruby linenos %} 
 #include <iostream>  
#include <string>  
using namespace std;  
int main()  
{  
   int a = 55;  
   double b = 65.123;  
   string str = "";  
   //头文件是sstream  
   std::ostringstream oss;  
   oss << a << "---" << b;  
   str = oss.str();  
   cout << str << endl;  
   return 0;  
}  
{% endhighlight %}

输出是55—65.123。

如果想实现小数点后只显示一位数字，使用下面方法：

{% highlight ruby linenos %} 
#include <iostream>  
#include <sstream>  
#include <iomanip>  
    
template <class T>  
std::string fmt(T in, int width = 0, int prec = 0) {  
    std::ostringstream s;  
    s << std::setw(width) << std::setprecision(prec) << in;  
    return s.str();  
}  
    
int main(){  
    std::string s = fmt(66.0 / 30.0, 2, 2);  
    std::cout << s << "\n";  
}  
{% endhighlight %}