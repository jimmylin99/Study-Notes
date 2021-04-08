**This is merely an (rough) archive, which is asynchronous to the original notes located in Notion**

**Last Synchronous Time (manually): 2021/04/07**

---

# Installation

## v1.16 (2021/02/16)

starting from v1.11 (2018/08/24), go introduces modules, so the usage of GOPATH is deprecated(?)

starting from v1.14 (2020/02/25), go officially says that modules are ready for production, all users are encouraged to migrate to modules from other dependency management systems

- **Ubuntu (20.04)**

  1. Download and put it in `/usr/local/go/bin`

     ```bash
     wget -c <https://dl.google.com/go/go1.6.linux-amd64.tar.gz> -O - | sudo tar -xz -C /usr/local
     ```

     *curl seems not work*

     *other arguments for tar might not work*

  2. Modify `PATH`

     add the following to `~/.zshrc` (`~/.bashrc` may be ok if no zsh installed), and source the file, or restart the shell

     ```bash
     export PATH=$PATH:/usr/local/go/bin
     ```

     */etc/profile cannot work for some unknown reason (ideally, for interactive shell, it should execute this file)*

     *it will only take effect for current user if setted in ~/.zshrc, haven't tried to apply it system-wide*

     Check the installation by

     ```bash
     go version
     ```

     *By default, `GOPATH` will be assigned to `~/go` since v1.8*, *there is even no need to use `GOPATH` after using modules since v1.11*

  3. Set proxy and Go Modules switch

     ```bash
     go env -w GO111MODULE=on
     go env -w GOPROXY=https://goproxy.io,direct
     ```

     *Not sure this feature works since which version, otherwise using export*

     *The proxy should worked in mainland China*

     The environment file for go is located under `$GOENV`

     There is another env variable called `$GOPRIVATE` for private repositories

- GoLand (Windows)

  Configure `GOROOT` to set the correct GO SDK

  If using Go Modules, there is no need to assign specified `GOPATH`; `GOPATH` cannot be in the same directory as `go.mod` as `GOPATH`

# Go Modules

- Get started

  ### Say you're writing a program called `simple`:

  1. Create a directory:

     ```bash
     mkdir simple
     cd simple
     ```

  2. Create a new module:

     ```bash
     # With this, Go creates a module root at that directory.
     go mod init github.com/username/simple
     # Here, the module name is: github.com/username/simple.
     # You're free to choose any module name.
     # It doesn't matter as long as it's unique.
     # It's better to be a URL: so it can be go-gettable.
     ```

  3. Put all your files in that directory.

  4. Finally, run:

     ```bash
     go run .
     ```

  5. Alternatively, you can create an executable program by building it:

     ```bash
     go build .
     
     # then:
     ./simple     # if you're on xnix
     
     # or, just:
     simple       # if you're on Windows
     ```

- Where is the package stored?

  Third-party packages are downloaded to `$GOPATH/pkg/mod`

panic & recover

闭包

切片：copy & append

- Receivers: value or pointer?

  The rule about pointers vs. values for receivers is that value methods can be invoked on pointers and values, but pointer methods can only be invoked on pointers. This is because pointer methods can modify the receiver; invoking them on a copy of the value would cause those modifications to be discarded.

  - example

    ```bash
    package main
    
    import "fmt"
    
    type Car struct {
        year int
        make string
    }
    
    func (c Car) String() string {
        return fmt.Sprintf("{make:%s, year:%d}", c.make, c.year)
    }
    
    func main() {
        myCar := Car{year: 1996, make: "Toyota"}
        fmt.Println(myCar)
        fmt.Println(&myCar)
    }
    ```

    ```bash
    {make:Toyota, year:1996}
    {make:Toyota, year:1996}
    ```

`init` 函数，每个源文件都可以有，在初始化声明后被调用

有些合法转换则会创建新值，如从整数转换为浮点数

类型转换，类型断言

### Go Concurrency Patterns

https://www.youtube.com/watch?v=f6kdp27TYZs

用channel通信，而不是直接用进程名通信；而且这个通信是**同步通信**（但buffered channel不具备同步性质）

fan-in pattern: multiplexing: channel to the channel

time-out pattern: select case

First function: with replicas

### Advanced Go Concurrency Patterns

https://www.youtube.com/watch?v=QDDwwePbDtw

panic 可以用来打印stack并查看goroutine leaks

go run -race <.go> # to detect race conditions

for-select loop (aformentioned)

### Concurrency is not Parallelism

TODO：https://talks.golang.org/2012/waza.slide#1

### Channel原理

runtime.hchan struct

FIFO

1. 元素：循环队列
2. 阻塞goroutine：2个双向链表
3. **mutex**

### P, M, G模型：goroutine是什么？与线程的区别

切换goroutine时，g_x→g0→g_y，保存PC和堆栈

## RPC

- 优点（相比于RESTful API）

  不需要额外定义接口；使用自定义协议，减少报文冗余；更高效的序列化协议；灵活性

sync.WaitGroup: Add, Done, Wait

### GoLand IDE的问题

go test找不到同一个包内的变量，则勾选 "Enable Go Modules integration" in GoLand under "Preferences" -> "Go" -> "Go Modules"

## 可寻址对象(addressable)

- [Go 语言规范](https://golang.org/ref/spec#Address_operators)中规定了可寻址(addressable)对象的定义：

  > For an operand x of type T, the address operation &x generates a pointer of type *T to x. The operand must be addressable, that is, either a variable, pointer indirection, or slice indexing operation; or a field selector of an addressable struct operand; or an array indexing operation of an addressable array. As an exception to the addressability requirement, x may also be a (possibly parenthesized) composite literal. If the evaluation of x would cause a run-time panic, then the evaluation of &x does too.对于类型为 T 的操作数 x，地址操作符 &x 将生成一个类型为 *T 的指针指向 x。操作数必须可寻址，即，变量、间接指针、切片索引操作，或可寻址结构体的字段选择器，或可寻址数组的数组索引操作。作为可寻址性要求的例外，x 也可为（圆括号括起来的）复合字面量。如果对 x 的求值会引起运行时恐慌，那么对 &x 的求值也会引起恐慌。For an operand x of pointer type T, the pointer indirection x denotes the variable of type T pointed to by x. If x is nil, an attempt to evaluate *x will cause a run-time panic.对于指针类型为 *T 的操作数 x，间接指针 *x 表示类型为 T 的值指向 x。若 x 为 nil，尝试求值 *x 将会引发运行时恐慌。

- 以下几种是可寻址的：

  - 一个变量: `&x`
  - 指针引用(pointer indirection): `&*x`
  - slice 索引操作(不管 slice 是否可寻址): `&s[1]`
  - 可寻址 struct 的字段: `&point.X`
  - 可寻址数组的索引操作: `&a[0]`
  - composite literal 类型: `&struct{ X int }{1}`

- 下列情况 `x` 是不可以寻址的，即不能使用 `&x` 取得指针

  - 字符串中的字节

  - map 对象中的元素

  - 接口对象的动态值(通过 type assertions 获得)

  - 常数

  - literal 值(非 composite literal)

  - package 级别的函数

  - 方法 method(用作函数值)

  - 中间值(intermediate value):

    - 函数调用

    - 显式类型转换

    - 各种类型的操作（除了指针引用 pointer dereference 操作 

      ```
      x
      ```

      ):

      - channel receive operations
      - sub-string operations
      - sub-slice operations
      - 加减乘除等运算符

- 例如，类型断言不可被寻址

  ```go
  l := list.New()
  l.PushBack(1)
  v := &l.Back().Value.(int) // error
  ```

  Solution:

  ```go
  // a)
  x := 1
  l.PushBack(&x) // notice that const cannot be addressable
  // b)
  b := l.Back()
  v := b.Value.(int)
  modify(&v)
  l.InsertAfter(v, b)
  l.Remove(b)
  ```

  十分复杂和麻烦，所以对于以interface{}为存储对象的数据结构，内部存储尽量使用指针（即方法a）

  对于list.List，参考StackOverflow说法和个人体验，建议使用slice代替

  另外，直接指针调用和unsafe.Pointer的速度相当，而使用interface{}（即需要type assertion）速度较慢6倍左右

------

- chan的左结合律：

  ```go
  var xx chan<-chan error // (chan<-) chan error
  var y chan(<-chan error)
  fmt.Println(xx == y) // compile error
  ```

------

### Variadic Functions

- Example

  ```go
  package main
  
  import "fmt"
  
  func sum(nums ...int) {
      fmt.Print(nums, " ")
      total := 0
      for _, num := range nums {
          total += num
      }
      fmt.Println(total)
  }
  
  func main() {
  
      sum(1, 2)
      sum(1, 2, 3)
  
      nums := []int{1, 2, 3, 4}
      sum(nums...)
  }
  ```

------

int to bool is not supported in the native language

switch 可以是任何类型，不一定整数或常量；fallthrough关键字

------

初始化行末尾要被逗号或右大括号包裹

- 该函数会死锁

  ```go
  func main() {
   ch1 := make(chan int)
   go fmt.Println(<-ch1)
   ch1 <- 5
   time.Sleep(1 * time.Second)
  }
  ```

  需要用闭包包裹，否则当作正常表达式处理；另实参会被先求值后调用，所以死锁

  - 因此下例也会死锁

    ```go
    go func(val int) {
      fmt.Println(val)
    }(<-ch)
    ```

------

### Pass by Value or Pass by Pointer

Strictly speaking, there is only one way in golang: pass by value.

- However, this does not mean that modifying non-pointer parameters will not affect the original data. In a very common case, when the variable is a struct which contains pointers as its members, then any modification via this (inner) pointer will influence the original data.
- Such kind of situation almost occurs everywhere within built-in structures, for example slice and map. So it is often says that these type of variables are passed by reference, which is of course not purely ture. This kind of point of view will result in problematic consequence when considering the non-pointer members since any change to its copy will not affect the original data, for example the length and capacity variable in slice. Even though this point of view is wrong, it is convenient to treat them as pass by reference since in most cases there is no need to pass a pointer to these types in order to modify its original data.

## Map 哈希表

声明：`var m map[string]int` m是零值，即map[string]int(nil)，实际内部是一个struct

初始化：`m := make(map[string]int)` or `m := map[string]int{}` or `m := map[string]int{"john":12, "tony":10}`

### Map的结构

简而言之，golang使用拉链法实现map；具体实现比较复杂

map的由hmap构成，hmap由若干个(2^B)个bmap（即bucket）和其它支持变量构成

每个bmap不超过8个键值对，否则使用溢出桶，当满足一些特定条件时触发扩容（其中之一是装载因子大于6.5，会扩容至原来的两倍；另一种是当有太多overflow桶时触发等量扩容，回收overflow buckets），扩容是非原子操作，在执行写操作时增量执行，所以不会有过大的瞬时性能波动；为保证内存的连续性，溢出桶接在所有bmap数组的后面

- 具体扩容步骤尚未研究，参考链接

  https://draveness.me/golang/docs/part2-foundation/ch03-datastructure/golang-hashmap/

一个有趣的事情是，当通过hash确定bmap（bucket）后，接下来在拉链里寻找k-v pair的方法，并不是直接比较key，而是比较top hash8位，这是考虑了比较uint8的速度比直接比较其它（更复杂的）的keytype要快；另外top hash 8位与决定是哪一个bucket的低位hash正好错开，大大降低了top hash冲突的概率

整体上，map的时间复杂度（均摊意义）是 O(1)

- runtime.hmap

  ```go
  type hmap struct {
  	count     int
  	flags     uint8
  	B         uint8
  	noverflow uint16
  	hash0     uint32
  
  	buckets    unsafe.Pointer
  	oldbuckets unsafe.Pointer
  	nevacuate  uintptr
  
  	extra *mapextra
  }
  
  type mapextra struct {
  	overflow    *[]*bmap
  	oldoverflow *[]*bmap
  	nextOverflow *bmap
  }
  ```

- gc.bmap

  ```go
  type bmap struct {
      topbits  [8]uint8
      keys     [8]keytype
      values   [8]valuetype
      pad      uintptr
      overflow uintptr
  }
  ```

- 一些结构图

  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/926c238a-a0da-4f8c-ad95-ad2040a8a8dd/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/926c238a-a0da-4f8c-ad95-ad2040a8a8dd/Untitled.png)

  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/786a80dd-5296-4f14-a109-d168fb4c0d46/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/786a80dd-5296-4f14-a109-d168fb4c0d46/Untitled.png)

### String

通常被视为只读的slice，内部结构为 `reflect.StringHeader` ，相比于slice少了个Cap

```go
type StringHeader struct {
	Data uintptr
	Len  int
}
// runtime.stringStruct
type stringStruct struct {
	str unsafe.Pointer
	len int
}
```

### String, Rune, Byte

- string, rune, byte, for-range

  string取单个字符返回的类型s[0]是byte (uint8)，单引号常量默认rune类型

  值得注意的是for-range返回的是rune(int32)类型，而不是byte

## Slice切片

一句不那么正确的话：golang中的引用类型包括 数组切片 map channel interface

以下来自官方博客：https://blog.golang.org/slices

- 但实际上golang只有pass by value，诸如slice的“引用”特性是语法糖导致的，slice相当于一个struct，内部大致包含了

  - 指针们：指向一个array的指针等

  - 非指针们：一个int表示数组长度等

  - 具体包含了（编译器维护了这样一个数组）

    ```go
    type SliceHeader struct {
    	Data uintptr
    	Len  int
    	Cap  int
    }
    ```

  所以即使这个struct被pass by value，copy出来的struct包含了相同的array指针，因此更改slice的内容会直接反映到原slice上

  - 但是长度却不会被更改，因此如果需要返回一个修改过长度的slice，需要显式地return

    ```go
    func SubtractOneFromLength(slice []byte) []byte {
        slice = slice[0 : len(slice)-1]
        return slice
    }
    
    func main() {
        fmt.Println("Before: len(slice) =", len(slice))
        newSlice := SubtractOneFromLength(slice)
        fmt.Println("After:  len(slice) =", len(slice))
        fmt.Println("After:  len(newSlice) =", len(newSlice))
    }
    
    // Before: len(slice) = 50
    // After:  len(slice) = 50
    // After:  len(newSlice) = 49
    ```

  更为直接地更改原slice的方法，就是传递slice指针

移除slice中元素的正确方法之一：

- 使用variadic arguments

  ```go
  func (s *Slice)Remove(value interface{}) error {
          for i, v := range *s {
              if isEqual(value, v) {
                  *s = append((*s)[:i],(*s)[i + 1:]...)
                  return nil
              }
          }
          return ERR_ELEM_NT_EXIST
  }
  ```

- 错误使用方法

  ```go
  slice := []int{1, 2, 3}
  newSlice := append(slice[:1], slice[2:]...)
  ```

  这会导致 slice 变成 [1, 3, 3]，因为append内部会将元素直接接到slice[1:]后面从而影响slice

  当然newSlice是正确的，因为makeslice会创建新的slice而不是原来的

这就引入了slice的append机制

### append

append分为两种情况，runtime会调用append(n *Node, inplace bool)

1. 非inplace情况

   - 即append(s, e1, e2, e3)，则

     ```go
     // file path: src/cmd/compile/internal/gc/ssa.go
     ptr, len, cap := s
     newlen := len + 3
     if newlen > cap {
         ptr, len, cap = growslice(s, newlen)
         newlen = len + 3 // recalculate to avoid a spill
     }
     // with write barriers, if needed:
     *(ptr+len) = e1
     *(ptr+len+1) = e2
     *(ptr+len+2) = e3
     return makeslice(ptr, newlen, cap)
     ```

   注意！如果newlen不大于cap，则不会growslice也就是说！ptr是原来slice的ptr，整个变化会直接影响原slice！这也就是为什么newSlice := append(slice[:i], slice[i+1:]...) 会影响slice的值，如果在for-range里这样做，则会直接影响该迭代第二次及以后的情况（当然如果直接return append(slice[:i], slice[i+1:])并且不再care参数slice的情况，那么可以直接return）

   为了保险起见，手动 tmpSlice := make([]someType, len(slice)) copy(tmpSlice, slice)，从而保障slice的不变（但这样写会比较冗长）

   - 或者使用append trick
     - append(slice, append([]int(nil), oldSlice...))

   不过值得注意的是copy操作的复杂度是O(n)，尤其是append(slice[:i], slice[i+1:]...)也是O(n)，如果只是为了删除元素且不考虑slice内元素的顺序，可以使用O(1)操作，slice[i] = slice[len(slice) - 1]

   ！另一种常见case：如果slice := []int{} for-range f(append(slice, x)) 则每次都会创建一个新的slice，这是因为slice在初始化的时候是一个len=cap=0的SliceHeader，而根据非inplace规则，每次append都溢出了cap，所以会通过growslice重新分配一个slice，因此操作不会影响原slice；而且growslice的cap会设置成newlen，也就是len(slice)+1，所以下一次添加又会溢出而继续分配一个新的slice，如此反复

   - 陷阱：如果原始slice被设置成 slice := make([]int, 0, 10) 则会出现严重的错误，因为在for循环里不会每次都分配新的slice了，从而影响从第二次迭代开始的append结果

2. 如果是inplace，其实跟not inplace一个思路，超过cap则一定会通过growslice分配一个新的ptr，如果不超过则会直接影响原slice

   但是，inplace额外保障了效率，也就是说不会在最后makeslice，所以对于不超过cap的情况，是不会发生大量内存拷贝的，因此有O(1)的均摊复杂度

   - 伪代码

     ```go
     // If inplace is true, process as statement "s = append(s, e1, e2, e3)":
     a := &s
     ptr, len, cap := s
     newlen := len + 3
     if uint(newlen) > uint(cap) {
        newptr, len, newcap = growslice(ptr, len, cap, newlen)
        vardef(a)       // if necessary, advise liveness we are writing a new a
        *a.cap = newcap // write before ptr to avoid a spill
        *a.ptr = newptr // with write barrier
     }
     newlen = len + 3 // recalculate to avoid a spill
     *a.len = newlen
     // with write barriers, if needed:
     *(ptr+len) = e1
     *(ptr+len+1) = e2
     *(ptr+len+2) = e3
     ```

参考题：

- 生成一个全排列

  for-range内部如上所述；值得注意的是，最开始的if中，不需要额外copy selected，因为每一个selected都是新分配的（也就是在 `f(append(selected, x), newRemained)` 的时候已经被copy并分配一个新slice了

  ```go
  func permute(nums []int) [][]int {
      ans := [][]int{}
      var f func(selected, remained []int)
      f = func(selected, remained []int) {
          if len(remained) == 0 {
              ans = append(ans, selected)
              return
          }
          for i, x := range remained {
              newRemained := make([]int, i)
              copy(newRemained, remained[:i])
              newRemained = append(newRemained, remained[i+1:]...)
  
              f(append(selected, x), newRemained)
          }
      }
      f(nil, nums)
  
      return ans
  }
  ```

概括：

如果是inplace，slice总是会被replace的，只需要关注被append的element的情况

如果不是inplace，则要注意如果newlen没有超过cap，append将直接作用在原slice上，可能会严重影响之后的结果；如果newlen超过了cap，则一切将发生在新分配的slice上（且新slice的cap=newlen）

一般而言，remove element的时候最好copy；而append在最后的时候不太需要copy

### Slice的index range

For arrays or strings, the indices are in range if 0 <= low <= high <= len(a), otherwise they are out of range. 所以允许s[len(s):]

## Interface

接口的出现是为了解决上下游过度耦合（依赖）的问题，相当于引入中间件，让上下游不再互相依赖对方的具体实现，而是只依赖统一的接口；它隐藏了底层实现，减少开发者所要关注的内容；接口的例子如POSIX定义了一串API、命令行等接口来建立应用程序与操作系统之间的交流。

因为语法层面单向隐式转换的原因，即在调用指针变量的方法时可以被隐式转换成被引用变量，所以指针变量可以调用non-pointer receiver的方法；反之不可以

Interface也是一种类型，具体来说在运行时是两种struct之一： `runtime.iface` & `runtime.eface` 后者表示不包含任何方法的空interface{}

- runtime.eface

  只包含指向底层数据和类型的两个指针

  ```go
  type eface struct { // 16 字节
  	_type *_type
  	data  unsafe.Pointer
  }
  ```

- runtime.iface

  ```go
  type iface struct { // 16 字节
  	tab  *itab
  	data unsafe.Pointer
  }
  ```

- runtime._type

  hash 字段能够帮助我们快速确定类型是否相等

  ```
  type _type struct {
  	size       uintptr
  	ptrdata    uintptr
  	hash       uint32
  	tflag      tflag
  	align      uint8
  	fieldAlign uint8
  	kind       uint8
  	equal      func(unsafe.Pointer, unsafe.Pointer) bool
  	gcdata     *byte
  	str        nameOff
  	ptrToThis  typeOff
  }
  ```

- runtime.itab

  fun 是一个动态大小的数组，它是一个用于动态派发的虚函数表，存储了一组函数指针。虽然该变量被声明成大小固定的数组，但是在使用时会通过原始指针获取其中的数据，所以 fun 数组中保存的元素数量是不确定的（没看懂这句话hhh）

  ```
  type itab struct { // 32 字节
  	inter *interfacetype
  	_type *_type
  	hash  uint32
  	_     [4]byte
  	fun   [1]uintptr
  }
  ```

接口不能表示任意类型，内部还是有保存底层类型即runtime._type，所以传递具体类型给interface{}类型参数时，会发生隐式转换，进而判断该参数与nil的关系会返回false

```go
type s struct{}
ni := interface{}(nil)
si := interface{}((*s)(nil))
fmt.Println(ni == nil, reflect.TypeOf(ni), reflect.ValueOf(ni))
fmt.Println(si == nil, reflect.TypeOf(si), reflect.ValueOf(si))
// Output:
// true <nil> <invalid reflect.Value>
// false *main.s <nil>
```

对于不能在编译器判断接口对应的具体类型（如果使用类型断言则可以在编译器确定），将会由运行时执行动态派发，这将会降低运行效率，但都是可以接受的（编译器会自动进行部分优化）

相比于动态派发的效率影响，不使用指针调用而是使用结构体，因此带来的内存拷贝，是更加耗时的，在开启动态派发的情况下，可以有一倍的差距（不同benchmark结果不同，且取决于结构体内存占用情况）

## Reflection 反射

- reflect.Type接口
- reflect.Value结构体

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/81e31026-c5e5-4903-8070-a47d036b2ba8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/81e31026-c5e5-4903-8070-a47d036b2ba8/Untitled.png)

- 第一原则： `interface{}` to `reflect.Type` & `reflect.Value`
- 第二原则：reflection Object to interface value
- 从接口值到反射对象：
  - 从基本类型到接口类型的类型转换；
  - 从接口类型到反射对象的转换；
- 从反射对象到接口值：
  - 反射对象转换成接口类型；
  - 通过显式类型转换变成原始类型；

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/66bb2463-71e1-4c5b-8ee8-eeafb56696e5/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/66bb2463-71e1-4c5b-8ee8-eeafb56696e5/Untitled.png)

- 第三原则：修改Value

## select语句

case 只能是channel收发操作

具备随机顺序的特性

- 阻塞式语法

非阻塞式语法

- 当包含default的时候，使用非阻塞式响应

可以选择 `x, ok := <-c` 实现非阻塞的收发

- 数据结构 `runtime.scase`

编译器对于select语句分为四种case：

- 空语句（直接转化成block函数）

- 单个case（近似转化成x, ok := ←c）

- 一个case 一个default （对唯一的一个case转化成非阻塞调用）

  `runtime.chansend` 可以传入一个参数，指明本次对channel的发送是非阻塞的（即如果channel被关闭或缓冲区满，就会立即返回而不是被阻塞）

- 多个case 会调用 `runtime.selectgo` 选择出要被执行的case的index，然后通过多个if执行对应的case

`runtime.selectgo` 会先确定两个顺序：轮询顺序和加锁顺序

`pollOrder` 是随机的 避免饥饿问题发生，保证公平性（饥饿指某些线程或（更小单位的）任务始终占据CPU而使得低优先级的线程无法获得（足够）的CPU时间执行）

`lockOrder` 是按照channel的地址排序的，避免发生死锁

在根据 `lockOrder`加速后，会根据 `pollOrder`轮询就绪的channel

- 如果存在就绪的，直接返回index
- 如果不存在，创建 `[runtime.sudog](<https://draveness.me/golang/tree/runtime.sudog>)` 结构体，将当前 Goroutine 加入到所有相关 Channel 的收发队列，并调用 `[runtime.gopark](<https://draveness.me/golang/tree/runtime.gopark>)` 挂起当前 Goroutine 等待调度器的唤醒；

当调度器唤醒当前 Goroutine 时，会再次按照 `lockOrder` 遍历所有的 `case`，从中查找需要被处理的 `[runtime.sudog](<https://draveness.me/golang/tree/runtime.sudog>)` 对应的索引

## defer语句

1. defer语句在return语句后执行

   - example

     ```go
     func c() (i int) {
         defer func() { i++ }()
         return 1
     }
     fmt.Println(c())
     
     // in the end the function return 2
     ```

2. 与 go 语句类似，参数会被先求值，所以如果要延迟求值，请使用闭包

   - example (negative)

     ```go
     func main() {
     	startedAt := time.Now()
     	defer fmt.Println(time.Since(startedAt))
     	
     	time.Sleep(time.Second)
     }
     
     // $ go run main.go
     // 0s
     ```

   - example (correct)

     ```go
     func main() {
     	startedAt := time.Now()
     	defer func() { fmt.Println(time.Since(startedAt)) }()
     
     	time.Sleep(time.Second)
     }
     
     // $ go run main.go
     // 1s
     ```

- 数据结构 `runtime._defer`

  通过 `link` 字段构成一个延迟调用的链表

  - `siz` 是参数和结果的内存大小；
  - `sp` 和 `pc` 分别代表栈指针和调用方的程序计数器；
  - `fn` 是 `defer` 关键字中传入的函数；
  - `_panic` 是触发延迟调用的结构体，可能为空；
  - `openDefer` 表示当前 `defer` 是否经过开放编码的优化；

  除了上述的这些字段之外，`[runtime._defer](<https://draveness.me/golang/tree/runtime._defer>)` 中还包含一些垃圾回收机制使用的字段，这里为了减少理解的成本就都省去了。

  ```go
  type _defer struct {
  	siz       int32
  	started   bool
  	openDefer bool
  	sp        uintptr
  	pc        uintptr
  	fn        *funcval
  	_panic    *_panic
  	link      *_defer
  }
  ```

  堆分配、栈分配和开放编码是处理 defer 关键字的三种方法，早期的 Go 语言会在堆上分配 runtime._defer 结构体，不过该实现的性能较差，Go 语言在 1.13 中引入栈上分配的结构体，减少了 30% 的额外开销，并在 1.14 中引入了基于开放编码的 defer，使得该关键字的额外开销可以忽略不计

  - 为什么栈比堆快？请参考[Techinal Pages](https://www.notion.so/0c8c203bd1a84c07bef42027990d5157)

- 开放编码

  Go 语言在 1.14 中通过开放编码（Open Coded）实现 `defer` 关键字，该设计使用代码内联优化 `defer` 关键的额外开销并引入函数数据 `funcdata` 管理 `panic` 的调用[3](https://draveness.me/golang/docs/part2-foundation/ch05-keyword/golang-defer/#fn:3)，该优化可以将 `defer` 的调用开销从 1.13 版本的 ~35ns 降低至 ~6ns 左右：

  ```
  With normal (stack-allocated) defers only:         35.4  ns/op
  With open-coded defers:                             5.6  ns/op
  Cost of function call alone (remove defer keyword): 4.4  ns/op
  ```

  然而开放编码作为一种优化 `defer` 关键字的方法，它不是在所有的场景下都会开启的，开放编码只会在满足以下的条件时启用：

  1. 函数的 `defer` 数量少于或者等于 8 个；
  2. 函数的 `defer` 关键字不能在循环中执行；
  3. 函数的 `return` 语句与 `defer` 语句的乘积小于或者等于 15 个

  Go 语言会在编译期间就确定是否启用开放编码

  开放编码在使用内联优化的同时，使用了延迟记录/延迟比特，在运行时记录哪些defer函数需要被最终调用（延迟比特位一共有8个）

  defer调用的函数及其参数会被存在 `cmd/compile/internal/gc.openDeferInfo` 结构体中

  如果 defer 关键字的执行可以在编译期间确定，会在函数返回前直接插入相应的代码，否则会由运行时的 runtime.deferreturn 处理

## Panic & Recover

### sort包

- sort.Sort

  快排混合堆排、希尔排序和插入排序

  如果size ≥ 12 则使用快排，但如果快排已经抵达深度 `2 * ceil(lg(n+1))` 则切换到更稳定（但常数更大的堆排，因为堆排序每次将root弹出，然后将heap尾替换root，再heapify the root，这个过程涉及的swap多了约10%，更重要的是相比于快排的局部性交换，heap sort对cache不友好

  如果size < 12 则先使用一次interval为6的希尔排序，即简单地将离得远的元素排序一下，然后再插入排序，从而减少插入排序因有太多过远的逆序对而大幅增加swap次数

  应用上，需要实现一个接口，它需要实现Len, Less, Swap，都是non-pointer receiver

```
sort.Ints`, `sort.Float64s`, `sort.Strings` implements `[]int`, `[]float64`, `[]string
```

what about `[]struct` or `[][]int`? Use `sort.Slice` , it automatically implements `Swap` and `Len` , what you need to do is to pass an anonymous function to the second parameter of this function, which plays a role in returning less, so the signature of the anonymous function is `func (i, j int) bool`

```
sort.Search(n, isSatisfied)` 支持二分查找，返回满足`isSatisfied == true` 最小的index，如果没找到返回 `n
```

### How to find source code for built-in functions?

- Question from StackOverflow

  Where in Go's source code can I find their implementation of `make`.

  Turns out the "code search" functionality is almost useless for such a central feature of the language, and I have no good way to determine if I should be searching for a C function, a Go function, or what.

  Also in the future how do I figure out this sort of thing without resorting to asking here? (i.e.: teach me to fish)

  **EDIT** P.S. I already found http://golang.org/pkg/builtin/#make, but that, unlike the rest of the go packages, doesn't include a link to the source, presumably because it's somewhere deep in compiler-land.

- Top rated answer

  There is no `make()` as such. Simply put, this is happening:

  1. go code: `make(chan int)`
  2. symbol substitution: `OMAKE`
  3. symbol typechecking: `OMAKECHAN`
  4. code generation: `runtime·makechan`

  `gc`, which is a go flavoured C parser, parses the `make` call according to context (for easier type checking).

  This conversion is done in [cmd/compile/internal/gc/typecheck.go](https://github.com/golang/go/blob/go1.10/src/cmd/compile/internal/gc/typecheck.go#L1831).

  After that, depending on what symbol there is (e.g., `OMAKECHAN` for `make(chan ...)`), the appropriate runtime call is substituted in [cmd/compile/internal/gc/walk.go](https://github.com/golang/go/blob/go1.10/src/cmd/compile/internal/gc/walk.go#L1417). In case of `OMAKECHAN` this would be `makechan64` or `makechan`.

  Finally, when running the code, said substituted function [in pkg/runtime](https://github.com/golang/go/blob/go1.10/src/runtime/chan.go#L70) is called.

  ### How do you find this

  I tend to find such things mostly by imagining in which stage of the process this particular thing may happen. In case of `make`, with the knowledge that there's no definition of `make` in `pkg/runtime` (the most basic package), it has to be on compiler level and is likely to be substituted to something else.

  You then have to search the various compiler stages (gc, *g, *l) and in time you'll find the definitions.