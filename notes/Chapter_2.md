# 第二章

## 1.main（）函數

main( ) 函数通常被啓動代碼調用，啓動代碼是由編譯器添加到程序中的，是程序和操作系統之間的橋梁。

實際函數頭描述的是main（）和操作系統之間的接口。

## 2.C++預處理器和iostream 文件

預處理器在進行主編譯之前對源文件處理。

如 #include <iostream> 該指令使預處理器將iostream文件的内容添加到程序中 。在編寫程序時我們大概率會用到這些程序和外部世界通信的輸入輸出工具。

實際上，#include<iostream>这条语句將被iostream文件内容給替換，隨源代碼一起發送給編譯器，源代碼文件和iostream組合成一個複合文件，在編譯的下一階段被使用。

\#include <> ： 直接到系统指定的某些目录中去找某些头文件。

\#include “” ： 先到源文件所在文件夹去找，然后再到系统指定的某些目录中去找某些头文件。

## 3.名稱空間namespace

名稱空間存在的意義在於當你在編寫大型程序時以及將多個廠商現有的代碼組合起來的程序更容易，有助於組織程序。例如在兩個都含有wanda（）函數的產品中，在使用時編譯器並不知道如何使用wanda（）的版本，而名稱空間讓廠商將產品封裝進一個叫做名稱空間的單元中，如Microflop ：：wanda（），Piscine：：wanda（）

值得一提的是，像iostream這樣的頭文件（包含文件），與傳統的C語言中的頭文件不盡相同，如math.h 和cmath，而不帶.h的頭文件可以包含名稱空間。原因在如iostream.h中，其定義的類和對象均在全局空間裏，而iostream的類和對象均在名爲std的名稱空間中，這也是能實現不同版本不同功能的原因。

應當注意的是string有點特別，C++在兼容C的標準庫時，C的庫中也有一個名爲string.h的頭文件，包含了常用的C字符串處理函數如strcmp、 这个头文件跟C++的string类半点关系也没有，他们(string.h和string)是毫无关系的两个头文件。

## 4.運算符重載與endl和’\n’

插入運算符”<<”是一個像按位左移運算符重載的例子，通過重載，使得同一個運算符有了不同的涵義，編譯器將通過上下文來確定運算符含義。

std::cout << std::endl;

 相當於

```c++
std::cout << '\n' << std::flush;
或者
std::cout << '\n'; std::fflush(stdout);
```

由於運算符的重載，對於‘\n’和“\n”是一樣的效果。都只實現了換行。

當輸出時，所輸出的内容會先進入緩存區，而flush（）的作用就是將緩存區的内容强行清空，以達到關閉輸入輸出時不會丟失數據。

对于有输出缓冲的流（例如cout、clog、），如果不手动进行缓冲区刷新操作，将在缓冲区满后自动刷新输出。

不过对于 cout 来说（相对于文件输出流等），缓冲一般体现得并不明显。但是必要情况下使用 endl 代替 '\n' 一般来说是个好习惯。

而对于无缓冲的流（例如标准错误输出流cerr），刷新是是非常不必要的，因爲flush（）是函數這意味著每次使用std：：endl時都會調用這個函數，過多的使用endl是程序執行效率低下的原因之一。

## 5.聲明語句和變量

計算機是一種精確的、有條理的機器。要將信息項存儲在計算機中，必須指出信息的存儲位置和所需的内存空間。如：int carrots 指出了需要的内存大小及内存單元的名稱。編譯器負責分配和標記内存的細節。Carrots為變量名，在C++中所有變量必須聲明，否則可能會造成其他無關的變量產生。定義聲明會使編譯器為變量分配内存空間；而引用聲明命令計算機使用在其他地方定義的變量。

## 6.函数和函数原型

函数原型之于函数就像变量之于声明语句一样，原型只描述了函数的接口。即函数与程序的其他部分交互的Gate,参数列表指出了何种信息将被传递给函数,函数类型指出了返回值的类型。

函数原型通常放于main（）函数前，函数定义放于main（）函数后。

C++不允许将函数定义嵌套在另一个函数定义中，每个函数定义都是独立的，所有函数的创建都是平等的。

对于main（）函数来说，不论是int 还是void ，它都是一个函数，誰又来调用的这个函数呢，这里操作系统可以看作是一个调用程序。

另外对于关键字，如果你不想把自己的编译器搞糊涂的话就不要用关键字(如int,main,namespace...)来作为变量名。

### 復習題：

#### 1.C++程序的模塊叫什麽？

模塊的概念是完成一個目的的函數集合

#### 2.  下面的預處理器編譯指令是做什麽用的？ 

\#include <iostream>

\#include代表引入iostream庫，該庫主要包含輸入輸出的類和對象。

#### 3.  下面的語句是做什麽的？

Using namespace std

所谓namespace，是指標識符的各种可见范围。

C++標準程序庫中的所有标识符都被定义于一个名为std的namespace中。因为标准库非常的庞大，所以程序员在选择的类的名称或函数名时就很有可能和标准库中的某个名字相同。所以为了避免这种情况所造成的名字冲突，就把标准库中的一切都放在名字空间std中。

 
