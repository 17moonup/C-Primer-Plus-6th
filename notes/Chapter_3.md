# 第三章

## 本章主要開始細述數據的處理(包含但不限於C++内置的類型,變量的命名規則,算術運算符,以及一種類型的數據怎麽變成另外一種數據類型)爲未來設計並拓展自己的數據類型奠定基礎，因爲所謂面向對象的編程，本質上就是讓數據和類型匹配，這樣才能更加方便地使用數據，面嚮對象編程也才有了意義:cactus:

## 1.声明和赋值

```c
int count; 
count =5;
```

這兩條語句表達了什麽意思?

:它正在存儲整數,並使用了一個名爲count的整型變量來表示他的值(5)

實際上,在這兩條語句運行時,具體的操作是,找一塊空閑的能夠存儲整型數據的16、24、32、甚至64位的内存,通常情況下系統都使用最小長度。可以通過下面的程序來查看自己的系統中不同長度的聲明,以及他們能表達最大值。

```c++
#include <iostream>
#include <climits>			//include the informations about the limits of int. use limit.h for older system
int main()
{
using namespace std;
int n_int = INT_MAX;
short n_short = SHRT_MAX;
long n_long = LONG_MAX;
long long n_llong = LLONG_MAX;
//size of return the length of type and variable
cout << "int is " << sizeof (int) << "bytes." << endl;
cout << "short is " << sizeof n_short << "bytes." << endl;
cout << "long is " << sizeof n_long << "bytes." << endl; //brackets are not needed
cout << "long long is " << sizeof (n_llong) << "bytes." << endl

cout << "Maximum values: " << endl;
cout << "int: " << n_int << endl;
cout << "short: " << n_short << endl;
cout << "long: " << n_long << endl;
cout << "long long: " << n_llong << endl;

return 0;
}
```

在理解了上述運行結果后，借力打力瞭解一下關於頭文件<climits> 裏的内容，之前已經在第二章裏簡述過有關C++的預處理器問題，知道了它是在程序編譯前就先對源代碼處理的文件，而且其文件内容會被替換到所在的行。在上面的源代碼中，<climits>文件裏定義了類似 #define INT_MAX 32767 的内容，而#define 和 Chapter_1中讲的#include 一樣是一個預處理編譯指令，它將會把源文件裏所有的INT_MAX 内容給替換成32767。這是c的符號常量，而在c++中有一種更加符合c++特點的創建符號常量方法——const，在稍後面再介紹，這裏只需要注意c和c++兼容問題即可。

------------------------------------------------------------------------------------------------------------------------------------------------

繼續有关數據類型的問題讨论,既然int在不同的計算機上可能會出現不同的位數,那麽可能出現的問題是,儅我在int為32位的計算機上編程了一個超過16位值的數據,然後在int為16位的計算機上運行,或者是在同一個計算機使用不同的編譯器時,可能就會造成[數據溢出]([C++栈溢出的原因及解决方法_GaoJieVery6的博客-CSDN博客_c++栈溢出的原因及解决办法](https://blog.csdn.net/WukongAKK/article/details/82559827?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-1-82559827-blog-120710408.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-1-82559827-blog-120710408.pc_relevant_default&utm_relevant_index=2))等無法挽回的錯誤。

另外，盡量在對變量聲明的時候就賦值，因爲分開看起來非常令人不爽:anger:

```c++
int a=0;
__________________________________________________________
int a;
a=0;
// right huh?
```

上面提到的整形數據都是帶符號的，它們也可以表示無符號數據，同時最大值也就能擴大一倍，如：unsigned short 可表示0-65535，對於無符號數（正數）在計算機内部的存儲都是原碼表示，<u>符號位</u>始終爲零，可進位，而有符號數符號位為1，大多數計算機以補碼的形式来表示負數。

```c++
#include <iostream>
#define ZERO 0
#include <climits>
int main()
{
using namespace std;
 	short sam =SHRT_MAX;
    unsigned short sue =sam;
    cout << "Sam has " << sam << "dollars and Sue has " << sue << endl;
    sam =sam+1;
    sue =sue+1;
    cout << "Sam has " << sam << "dollars and Sue has " << sue << endl;
    sam =ZERO;
    sue =ZERO;
    cout << "Sam has " << sam << "dollars and Sue has " << sue << endl;
    cout << "Sam has " << sam << "dollars and Sue has " << sue << endl;
    sam =sam -1;
    sue =sue -1;
    cout << "Sam has " << sam << "dollars and Sue has " << sue << endl;
    cout << "Sam has " << sam << "dollars and Sue has " << sue << endl;
    return 0;
}
```

通過上面的程序可以發現，出現儅數據超越本身的類型限制時，c++會將值設爲另一端的取值，这是c和c++针对溢出的[保护机制和经典的栈溢出攻击](https://cloud.tencent.com/developer/article/1765340)，但并不保證這種行爲不會出錯。

### 例如：

#### 整型數據溢出導致死循環

```c++
short i =0;
...
while(l< MAX_L) 		//MAX_L代表一个比较大的整形
{
l += readFromInput(fd,buf);
buf += l;
}
```

#### 整形转型時的溢出

```c++
int copy_something(char *buf, int len)
{
    #define MAX_LEN 256
    char mybuf[MAX_LEN];
     ... ...
     ... ...

     if(len > MAX_LEN){ // <---- [1]
         return -1;
     }

     return memcpy(mybuf, buf, len);
}
```

在if语句处，len是一个有符号的int整形，而memcpy函数的参数要求是一个size_t的无符号的类型，

于是len会被提升为unsigned，在这时如果我们给len一个负数值，可以满足if判断条件，但在memcpy会成为一个正数，于是mybuf就会overflow，这里导致的结果是mybuf缓冲区后面的数据被重写。

在csapp上第二章也有相似的例子。另外可能会有一些关于[buffer](https://blog.csdn.net/S_o_l_o_n/article/details/102518959)和[cache](https://www.cnblogs.com/mlgjb/p/7991903.html)的一些问题

#### 分配内存

关于整数溢出导致堆溢出的很典型的例子是，OpenSSH Challenge-Response SKEY/BSD_AUTH 远程缓冲区溢出漏洞。下面这段有问题的代码摘自OpenSSH的代码中的auth2-chall.c中的input_userauth_info_response() 函数:

```c
nresp = packet_get_int();
if (nresp > 0) {
    response = xmalloc(nresp*sizeof(char*));
    for (i = 0; i < nresp; i++)
        response[i] = packet_get_string(NULL);
}
```

上面这个代码中，nresp是size_t类型（size_t一般就是unsigned int/long int），这个示例是一个解数据包的示例。一般来说，数据包中都会有一个len，然后后面是data。

如果我们精心准备一个len，比如：1073741825（在32位系统上，指针占4个字节，unsigned int的最大值是0xffffffff，我们只要提供0xffffffff/4 的值——0x40000000，这里我们设置了0x4000000 + 1）， nresp就会读到这个值，然后nresp * sizeof(char * )就成了 1073741825 * 4，于是溢出，结果成为了 0x100000004，然后求模，得到4。于是，malloc(4)，于是后面的for循环1073741825 次。

经过0x40000001的循环,用户的数据早已覆盖了xmalloc原先分配的4字节的空间以及后面的数据，包括程序代码，函数指针，于是就可以改写程序逻辑。关于更多的东西，你可以看一下这篇文章[《Survey of Protections from Buffer-Overflow Attacks》](https://www.academia.edu/60654203/Survey_of_Protections_from_Buffer_Overflow_Attacks)）。

#### size_t 的溢出

```c
for (int i= strlen(s)-1;  i>=0; i--)  { ... }
for (int i=v.size()-1; i>=0; i--)  { ... }
```

上面这两个示例是我们经常用的从尾部遍历一个数组的for循环。第一个是字符串，第二个是C++中的vector容器。strlen()和vector::size()返回的都是 size_t，size_t在32位系统下就是一个unsigned int。

你想想，如果strlen(s)和v.size() 都是0呢？这个循环会成为个什么情况？于是strlen(s) – 1 和 v.size() – 1 都不会成为 -1，而是成为了 (unsigned int)(-1)，一个正的最大数。导致你的程序越界访问。

这样的例子有很多很多，这些整型溢出的问题如果在关键的地方，尤其是在搭配有用户输入的地方，如果被黑客利用了，就会导致很严重的安全问题。

以上都是一些數據由於不經意的定義而產生的内存溢出，在不同的情況下會出現不同的結果。但主要的邏輯還是通過c和c++對與數據的保護而產生的溢出從而導致的各種可以選擇的情況，所以速度和安全還是不可兼得的。

[更多有關編譯器處理這些的内容引用原文地址附在這](https://coolshell.cn/articles/11466.html#%E4%BB%80%E4%B9%88%E6%98%AF%E6%95%B4%E5%9E%8B%E6%BA%A2%E5%87%BA)

另外,有關這樣的一行代碼:

```c++
cout << "Year " << 1492 << endl;
```

c++的編譯器是如何確定Year類型的?

除非帶有後綴,或者數據太大,否則c++編譯器統統將整型變量以int處理(速度最快,性能最好)

#### 最後介紹一個整型:char

char是專爲存字母等字符類型的數據而為設計的整型,字母的存儲實際還是通過編碼轉換成了數字的存儲(如在美國最常用的符號集ASCII字符集),因此也可以將char用作比short更小的整型。

char也是有signed char and  unsigned char的,但在默認情況下,它既不是無符號也不是有符號(薛定諤的貓 :?),c++把權限給編譯器開發人員。Normally, 當你用char表示字符時有沒有符號都沒關係， 但當你使用char來表示數字的時候，那就要像對待上面int的整型溢出一樣認真對待char了。

#### const類型符

```c++
``` const type name = value;
___________________________________________________________________________________________
    const int foo ;//the value of foo is undefined.
	foo =10;// foo has been defined const 
```

const 在定義數據時和c的#define一樣, 但是const可以用於數組和結構,而且明確了數據類型。

### 浮點數

可以表示如３.１４１５９２６５３５的小數

在計算機内部分爲值和對值進行放大和縮小的兩部分存儲,就像314和3,14值是一樣,只是縮放因子不一樣,這裏所謂縮放因子的意思就是浮點數的意思。

#### 浮點類型

- float 		（至少32位）
- double     （至少48位）
- long double  （80、96、128）

三種是按其有效位數和允許的指數最小範圍分類的

cout在輸出時通常會省略掉沒有意義的0

另外提一下後綴名

```c++
1.234f	//float
1.234e2	//double
1.1L    //long double
```

和符號整型數據相比，浮點數顯然可以表示更大更細的範圍，但運算速度會慢，而且精度會降低

```c++
//floatadd.cpp  --precision with float
#include <iostream>
int main()
{
using namespace std;
float a =2.34E+22f
float b =a+1.0f
cout << "a= " << a << endl;
cout << "b-a ： " << b-a << endl;
return 0;
}
```

Summary：

浮點常量在默認情況下為double類型

整型：

- 符號整型：signed char、short、int、long、longlong、bool
- 無符號整型：unsigned char、short、unsigned int 、long

浮點型：

- float
- double
- long double

### 算數運算符

這裏除了加減乘除外多了一個求模%，也就是取餘數

如：14%3，只取第一個數除第二個數的餘數，所以只對整型數據有效，浮點數求模會出錯。

對於浮點數求模用的是一個fmod（）函數，定義在<cmath>頭文件中，可以去查看。

#### 運算符的優先級和結合性

先乘除后加減，結合性相同情況下從左到右

#### 類型轉換

- 將一種算術類型的值賦給另一種算數類型的變量，c++會對值進行轉換
- 表達式中包含不同的類型，c++會對值進行轉換
- 將參數傳遞給函數時，c++對值進行轉換

之前討論過潛在的類型轉換產生的bug

對於浮點數來説，將大的浮點數換為小的浮點數，如double換成float，精確度會降低，值也有可能超過目標類型的取值。

若將浮點數換成整型，小數部分將丟失，值可能超出。

前兩種情況都會產生不確定的結果。

以{}進行的初始化稱爲列表初始化，這種初始化對類型轉換比較嚴格，需細心對待。

在傳遞參數時的類型轉換由c++函數原型控制

强制類型轉換 static_cast<>

