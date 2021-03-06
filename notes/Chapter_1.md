# 第一章

## 1.過程性編程和面向對象編程的區別

要知道首先出現的是過程性編程（確定計算機應采取的操作，然後使用編程語言去實現這些操作——即强調算法方面）

而面向對象編程强調對數據，讓語言滿足問題的要求。

類是一種規範，它描述了與問題的本質特性相對應的數據格式及處理方式，而對象是根據類而構造的特定數據結構。

類可以來自類庫，他們沒有被内置到C++語言中，而是語言標準指定的類，//這些類定義iostream文件裏，而實際上C/C++之所以attractive很大程度上是由於存在大量支持UNIX、Macintosh和Windows編程的類庫。

所以設計有用高效可靠的類是Key point，在現在編程中大多要使用的類已在類庫裏提供。

C++的優勢正在與可以方便的修改和重用現有的代碼。

## 2.C++是如何在C語言的基礎上添加面向對象概念的

20世紀70年代，肯·湯姆森為了使其設計的Unix系統更加高效，使用B語言的變種（即C語言）在DEC PDP-7電腦上重寫了Unix。

C語言簡潔，與UNIX操作系統聯係緊密，C++的OOP方面是受到了Simula67的啓發。

OOP部分賦予了C++語言將問題所涉及的概念聯係起來的能力，C部分則賦予了C++語言緊密聯係硬件的能力。

3.C++是如何在C語言的基礎上添加汎型編程的

泛型編程的目的也和OOP一樣——使通過重用代碼和抽象通用概念的技術更加簡單

而和OOP的區別是：OOP更强調編程的數據方面，汎型編程更强調特定的數據類型。

在C++獲得一定程度成功后，Stroustrup才添加了模板使得汎型編程出現。
