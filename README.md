# 2024090914022-赖伊卡-CS-03
## C语言内存模型
### 概念理解
#### 1,程序代码区
具体含义：程序代码区存放了程序的可执行代码，包括函数、指令等。这部分内存是只读的，防止程序运行时被意外修改。

应用场景：当程序被加载到内存中时，其可执行代码被放置在代码区。CPU通过执行这些代码来执行程序的功能。

调用方式：程序代码区的执行不需要显式调用，而是由操作系统或程序自身的执行流程自动执行。程序中的函数调用会导致CPU跳转到代码区的相应位置执行。

#### 2,常量区
具体含义： 常量区用于存储程序中定义的常量值，如字符串常量、数字常量等。这些值在程序运行期间不会改变。

应用场景： 常量区常用于存储全局常量，这些常量在程序的多个部分中被引用，但不允许被修改。例如，配置信息、错误代码等。

调用方式： 常量区的访问通常通过变量名或常量名进行，编译器在编译时会将常量名替换为对应的常量值。

#### 3,全局数据区
具体含义： 全局数据区用于存储全局变量和静态变量。这些变量在程序的整个运行期间都存在，并且对所有函数都是可见的。

应用场景： 全局数据区常用于存储需要在程序多个部分之间共享的数据。例如，配置文件中的参数、程序的运行状态等。

调用方式： 全局数据区的变量可以通过变量名直接访问，无需特殊调用方式。

#### 4,堆区
具体含义： 堆区是程序运行时用于动态分配内存的区域。与栈区不同，堆区的内存分配和释放由程序员手动控制（通过如malloc、free等函数）。

应用场景： 堆区适用于存储大小不确定或生命周期与程序运行时间相同的数据。例如，动态数组、链表、树等数据结构。

调用方式： 堆区的内存分配通常使用如malloc、calloc、realloc等函数，释放则使用free函数。

#### 5,动态链接库
具体含义： 动态链接库是一种包含可被多个程序同时使用的代码和数据的文件。与静态链接库不同，动态链接库在程序运行时才被加载到内存中。

应用场景： 动态链接库常用于实现代码的共享和重用，减少程序的大小和提高内存的使用效率。例如，操作系统的API、图形库、数据库驱动等常以动态链接库的形式提供。

调用方式： 在Windows系统中，动态链接库的调用可以通过隐式链接或显式链接来实现。

#### 6,栈区
具体含义： 栈区是程序运行时用于存储局部变量、函数参数和返回地址等数据的区域。栈区遵循后进先出的原则。

应用场景： 栈区主要用于函数调用的过程中，存储函数的局部变量、参数和返回地址等信息。每次函数调用时，都会在栈上分配一块内存用于存储这些信息，函数返回时则释放这块内存。

调用方式： 栈区的使用由编译器自动管理，程序员无需显式调用。函数调用和返回时，编译器会自动在栈上分配和释放内存。

### 导学模型
#### 1. 
  指程序在运行过程中，由于栈空间的使用超过了其分配的大小，导致数据溢出到其他内存区域。这种情况通常会导致程序崩溃，甚至可能被恶意利用来执行未授权的代码。
#### 2.
      1,内存分配与释放
      栈区：由操作系统自动分配和释放，存放函数的参数值、局部变量的值等。在函数调用时，系统为这些局部变量在栈上分配空间，函数返回时，系统自动释放这些空间。
      堆区：一般由程序员手动申请分配和释放。程序员使用如malloc、new等函数或操作符在堆上申请空间，使用完毕后需通过free、delete等函数或操作符手动释放空间。若程序员不释放，程序结束时可能由操作系统回收。

      2,内存大小和空间
      栈区：栈的大小系统预先规定好的，空间相对较小。在Windows下，栈的大小通常是2MB（也有说法是1MB），这是一个编译时就确定的常数。如果申请的空间超过栈的剩余空间，将提示栈溢出。
      堆区：堆的大小受限于计算机系统中有效的虚拟内存，空间相对较大，且比较灵活。堆申请的内存是一块不连续的内存区域，这是由于系统使用链表来存储空闲内存地址。
  
      3,数据结构和存取方式
      栈区：栈是一种先进后出的数据结构，只允许在栈顶进行插入（push）和删除（pop）操作。栈中的数据在内存中是一块连续的区域。
      堆区：堆虽然也常被看作是一种树形数据结构（如堆排序中的堆），但在内存管理中，堆更多是指一种动态分配内存的方式。堆中的数据在内存中是不连续的区域，由链表等数据结构管理。
   
      4,缓存与访问速度
      栈区：栈使用的是一级缓存，调用速度较快。栈中的数据在调用时通常处于存储空间中，调用完毕后立即释放。
      堆区：堆是存放在二级缓存中的，生命周期由虚拟机的垃圾回收算法来决定。因此，调用堆中对象的速度相对较慢。
   
      5,碎片与内存管理
      栈区：栈不会产生内存碎片，因为它是连续分配的内存区域。
      堆区：堆由于采用链表等数据结构管理内存，容易产生内存碎片。碎片是分配出去但未被使用的内存空间，它们虽然存在但无法被有效利用。
   
      6,应用场景
      栈区：适用于存储局部变量、函数参数等生命周期较短的数据。由于栈的自动分配和释放特性，以及LIFO的存取方式，栈在函数调用、递归算法等场景中有着广泛的应用。
      堆区：适用于存储生命周期较长或大小不确定的数据，如动态数组、链表、树等数据结构。堆的灵活性使得它成为实现这些数据结构的重要选择。
  #### 3.
  只读区域有程序代码区，常量区。
  
  可读写区域有全家数据区，堆区，动态链接库和栈区。
  #### 4.
  malloc可以动态分配内存，原型为void* malloc(size_t size)。也就是括号里给定你要开辟的内存大小，前面用指针变量接收就可以了。
  
  free则是用来释放掉malloc分配的内存块，原型是void free(void* ptr);也就是直接在括号里给出要释放空间的地址就行。
  
  他俩都是在堆区上进行操作的。
  #### 5.
      1，避免内存泄露，内存管理可以及时释放不再使用的内存，防止内存泄漏的发生。
      2，提高内存使用效率，内存管理可以优化内存空间的使用更高效的利用有限的内存空间。
      3，提高程序的执行速度，内存管理可以避免内存之间发生冲突，从而使程序可以更加高效的进行。
      4，保证程序的安全与稳定，防止诸如野指针，越界访问等导致程序崩溃或者故障。

      
## 内存模型的运用

首先，我想先通过各个类型的含义来推出其在内存中的位置。

      constValue:常量存储区
	    因为他的值在编译时就已确定好了，并在整个程序内保持不变。假设它不在常量存储区，那么它可能会被意外修改，从而破坏了程序的预期行为。
     
      constString：全局数据区
	    因为它是在函数外被定义的。
            这里我想说明一下const，因为const是在*之前，所以不可被更改的是*constsring而非其本身。
     
      globalVar:全局数据区
      因为它在程序的整个生命周期内都有效，并且在程序开始执行之前就已经被分配了存储空间。
      
      staticVar：全局数据区
      在这里虽然他是在main函数内部被定义，看似应该是在栈区，但由于static关键字，它的存在周期贯穿整个程序。
      
      localVar:栈区
      localVar是function函数内部的一个局部变量，它在函数被调用时创建，在函数返回时销毁。
      
      localVarMain:栈区
      与localvar类似 只不过它在主函数内。
      
      ptr:栈区
      ptr是一个指向整数的指针变量，它本身作为局部变量存在

我也另外给出了一种方法。我分别对每个变量取了地址。结果如图所示：
![image](https://github.com/Taylor-Lai/2024090914022-laiyika-CS-03/blob/main/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-23%20234326.png)

根据图片可以很清楚的看到constvalue单独位于一个分区，constsring,globalvar,staticvar.位于同一分区，而locavar,ptr,localvarmain,位于同一分区，这恰好佐证了先前的论断。

## 浅谈Cache
1.冯诺伊曼结构有以下几点

      1，冯诺伊曼结构采用了二进制的形式表示数据和指令。因为计算机只有高电压和低电压两种信号之分。
      2，计算机分为五个基本组成部分：运算器、控制器、存储器、输入设备和输出设备。
      3，程序和数据都存储在同一个存储器中，计算机按照存储在存储器中的指令逐条取出指令并自动执行。
pl，此外我有了解到在冯体系之外，存在另一个哈佛体系。其与冯的最大区别在于第三条哈佛体系的指令存储器和数据存储器是分开的。相较于冯体系有利有弊，显然，他的处理速率会更快（毕竟可以同时访问指令和数据）。但同时，其结构更加复杂，成本也更高。（个人以为这也就是现在冯体系占主导地位的原因。）
      
现代计算机的组织结构是指计算机硬件系统的构成方式，比如说单处理器系统，多处理器系统，分布式系集群系统等等。

关于两者的不同，我更愿意把它理解为现代结构在冯体系上的提升与优化，因为本质上来说现在的计算机结构也基本是冯结构的完善与改进（虽然一直有突破冯瓶颈的目标存在，但并没有改变冯结构“垄断”的事实），类似于后面问题中提到的缓存，就是一个在冯体系下提高运行速率的典型例子。

不同点有以下几点

      1，现代计算机以存储器为中心，更加注重数据的快速访问。而冯体系，则是以运算器为核心，感觉所有部件都是围着运算器在转。
      2，冯体系中运算器和控制器独立的两个部件，而现在则将两者集成形成所谓的cpu。
      3，	冯体系只能严格的采用顺序执行指令。而现代结构采用了流水线技术等大大提高了指令执行的效率。
      
对于冯诺伊曼结构的学习，最初是在计算机导论课上。给我最初的感受是一种震惊吧。一直以来看起来极端复杂的计算机系统，原来归根结底如此简单，甚至可以用朴素来形容。这也给了我一种思想，也就是在问题变得愈发复杂之时，跳出来,好好的审视一下问题本身，以及自己大体的思路，不要被绕进去。就像我最开始做后面的大数运算的题时，开始真的被绕的很晕。但是后面我就仔细考虑了一下我要解决问题的思路，需要创建哪些函数。这样来看，原本一两百行的代码其实也没多少东西hh。

2.
 读操作

      1.	CPU发送读指令和地址：CPU需要读取数据时，会向主存储器发送读指令，并通过地址线将需要读取的数据的地址传递给MAR。
      2.	MAR传递地址给存储体：MAR接收到地址后，会将其传递给存储体，指示存储体去寻找对应地址的数据。
      3.	存储体读取数据并传递给MDR：存储体根据MAR提供的地址，找到对应的数据并将其通过数据线传递给MDR。
      4.	MDR将数据返回给CPU：MDR接收到数据后，会将其暂存起来，并等待CPU来取。CPU通过数据线从MDR中读取所需的数据。
  写操作
  
      1.	CPU发送写指令、数据和地址：CPU需要写入数据时，会向主存储器发送写指令，并通过数据线将需要写入的数据传递给MDR，同时通过地址线将目标地址传递给MAR。
      2.	MDR和MAR传递信息给存储体：MDR将需要写入的数据传递给存储体，MAR将目标地址也传递给存储体。
      3.	存储体写入数据：存储体根据MAR提供的地址，找到对应的存储单元，并将MDR提供的数据写入该存储单元中。
  
其实内存的运行原理不难理解，我在学的时候，总结成了，cpu要什么，他就给什么。配合cpu的行动就行了。


3.

Cache的局部性原理包括时间局部性和空间局部性。

      时间局部性原理指的是，如果一个数据或者指令在最近的过去被访问过，那么它在不远的将来也很可能再次被访问。因为程序中常常包含循环结构，从而表现出很高的时间局部性。此外，一些经常使用的变量、函数返回值等也可能在短时间内被多次访问，这也体现了时间局部性的原理。
      
      空间局部性原理指的是，在最近的将来要用到的数据的地址和现在正在访问的数据地址很可能是相近的。这主要是因为程序通常是顺序执行的，因此相邻的指令和数据项往往会被连续访问。此外，数组、结构体等数据结构中的元素在内存中也是连续存储的，因此当访问其中一个元素时，其相邻的元素也很可能很快被访问。 
      
Cache能够存储那些最有可能被CPU访问的数据和指令，从而减少对主存的访问次数，提高程序的执行速度。具体来说，Cache利用时间局部性和空间局部这些数据或指令时，可以直接从Cache中快速获取，而不需要再去访问速度较慢的主存。这种机制大大提高了CPU的访存速度，从而提升了整个计算机系统的性能。

在学习缓存的时候，我最先联想的是我的输入法，我常用的字每次都能在前列跳出来。我觉得缓存真的很妙，它能够保存一些后面cpu大概可以用到的指令或者数据，减少cpu对于内存的“打扰”。现实中，手机上如果没有“后台关闭”
那你打开他就非常快，这应该也是相似的原理。

