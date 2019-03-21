# Go Note

## 随学随记
1. Go是一种非常严格的语言，它几乎总是要求我们"以标准答案去答题"，在其它语言可以容忍的不规范编码方式在Go语言中几乎都会抛异常。

2. 静态语言意味着变量必须要指定数据类型(int,string,bool,[]byte等)。虽然必须指定数据类型，但除了在声明变量的时候显式指定数据类型，也可以让Go自己去推断数据类型.

3. **go run命令同时进行了编译和运行两个过程：它将使用一个临时目录保存构建的程序，然后执行它，最后自动清理构建出来的临时程序。**

4. 在开发阶段，用go build还是用go run，随意即可。但在部署的时候，一般先go build，再go run。

5. **在Go中，程序的入口是main包中的main函数，这两名称都是固定的。**

6. 此外，还可以构建本地的网页版官方手册，在断网的时候可以访问,然后就可以在浏览器中通过http://localhost:6060/访问官方手册。：

  > godoc -http=:6060

7. `:=` 在Go中属于类型推断操作，它包含了变量声明和变量赋值两个过程。`:=`只能在函数代码块内部使用，在全局作用域下使用将报错，因为类型推断是在运行期执行的，而全局范围内的变量**声明部分**是在编译期间就决定好的。

8. 需要注意，name第二次被:=赋值，Go第一次推断出该变量的数据类型之后，就不允许:=再改变它的数据类型，因为只有第一次:=对name进行声明，之后所有的:=对name都只是简单的赋值操作。

  ```go
  func main(){
      name,age := "longshuai",23
      fmt.Println("name:",name,"age:",age)
      
      // name重新赋值，因为有一个新变量weight
      weight,name := 90,"malongshuai"
      fmt.Println("name:",name,"weight:",weight)
  }
  ```

9. 有些时候，我们可能并不需要所有的返回值。例如，某个函数有两个返回值，我们只想要取得第二个返回值。这时可以将不想要的返回值丢给特殊符号下划线_，它表示丢弃这部分结果，如下：

  ```go
  func test(x int, y string) (int, bool){
  	code...
  }
  
  // 只取第二个布尔类型的返回值
  _, status := test(1,"test")
  
  ```

10. Go语言不是一门面向对象的语言，没有对象和继承，也没有面向对象的多态、重写相关特性。Go所拥有的是数据结构，它可以关联方法。

11. 定义一个数据结构

   ```go
   // 定义一个结构体
   type Animal struct {
       name string
       speak string
   }
   
   bm_horse := Animal{
       name:"baima",
       speak:"neigh",
   }
   ```

12. 指针

   > 	bm_horse := Animal{}表示返回一个数据结构给bm_horse，bm_horse指向这个数据结构，也可以说bm_horse是这个数据结构的引用。
   	除此，还有另一种赋值方式，比较下两种赋值方式：

   	```go
   	1.bm_horse := Animal{"baima","neigh"}
   	2.ref_bm_horse := &Animal{"baima","neigh"}
   	```
   > 	这两种赋值方式，有何不同？:=操作符都声明左边的变量，并赋值变量。赋值的内容基本神似：
   >
   > 	第一种将整个数据结构赋值给变量bm_horse，bm_horse从此变成Animal的实例；
   >	
   > 	第二种使用了一个特殊符号&在数据结构前面，它表示返回这个数据结构的引用，也就是这个数据结构的地址，所以ref_bm_horse也指向这个数据结构。
   >	
   > 	那bm_horse和ref_bm_horse都指向这个数据结构，有什么区别？
   >	
   > 	实际上，赋值给bm_horse的是Animal实例的地址，赋值给ref_bm_horse是一个中间的指针，这个指针里保存了Animal实例的地址。它们的关系相当于：

   ```go
    	1.bm_horse -> Animal{}
   	2.ref_bm_horse -> Pointer -> Animal{}
   ```
   >	**其中Pointer在内存中占用一个长度为一个机器字长的单独数据块，64位机器上一个机器字长是8字节**，所以赋值给ref_bm_horse的这个8字节长度的指针地址，这个指针地址再指向Animal{}，而bm_horse则是直接指向Animal{}。

13. **Go函数给参数传递值的时候是以复制的方式进行的**。

    因为复制传值的方式，如果函数的参数是一个数据结构，将直接复制整个数据结构的副本传递给函数，这有两个问题：

    1. 函数内部无法修改传递给函数的原始数据结构，它修改的只是原始数据结构拷贝后的副本
    2. 如果传递的原始数据结构很大，完整地复制出一个副本开销并不小

14. Go中有**两种类型的包**，或者说有两种类型的文件：

      1. 编译后成为可执行文件的包，也就是main包编译后的得到的文件

   15. 编译后成为共享库的包，只要go程序文件中声明的不是main包，就是库文件

16. Go通过名称首字母的大小写决定属性是否允许导出：

    - 首字母大写的属性是允许导出的属性
    - 首字母小写的属性不允许被导出

17. 工作空间(workspace) 

   * 通过环境变量`GOPATH`设置workspace的路径

    * Go编程人员一般将它们的Go代码放在一个`workspace`下，当然，这不是必须的
    * workspace包含一个或多个版本控制系统的仓库(如git)
    * 每个仓库包含一个或多个package
    * 每个package由单个目录下的一个或多个Go源文件组成，它们都必须声明目录名作为它们的包名
    * package的目录路径决定导入包时import的路径

18. Go 的导包，编译器会从相对路径搜索，即从GOROOT或GOPATH(workspace)下的src下开始搜索，go总是先从`GOROOT`出先搜索，再从`GOPATH`列出的路径顺序中搜索，只要一搜索到合适的包就理解停止。当搜索完了仍搜索不到包时，将报错。

19. 首先从main包开始，如果main包中有import语句，则会导入这些包，如果要导入的这些包又有要导入的包，则继续先导入所依赖的包。重复的包只会导入一次，就像很多包都要导入fmt包一样，但它只会导入一次。

   每个被导入的包在导入之后，都会先将包的可导出函数(大写字母开头)、包变量、包常量等声明并初始化完成，然后如果这个包中定义了init()函数，则自动调用init()函数。init()函数调用完成后，才回到导入者所在的包。同理，这个导入者所在包也一样的处理逻辑，声明并初始化包变量、包常量等，再调用init()函数(如果有的话)，依次类推，直到回到main包，main包也将初始化包常量、包变量、函数，然后调用init()函数，调用完init()后，调用main函数，于是开始进入主程序的执行逻辑。

20. go要求import导入的包必须在后续中使用，否则会报错。如果想要避免这个错误，可以在包的前面加上下划线：

   ```go
   import (
       "fmt"
       _ "net/http"
       "mypkg"
   )
   ```

   这样在当前包中就无需使用`net/http`包。其实这也是为包进行命名，只不过命名为"_"，而这个符号又正好表示丢弃赋值结果，使得这成为一个匿名包。

   > **下划线(_)**
   > 在go中，下划线出现的频率非常高，它被称为blank identifier，可以用于赋值时丢弃值，可以用于保留import时的包，还可以用于丢弃函数的返回值。

   导入而不使用看上去有点多此一举，但并非如此。因为导入匿名包仅仅表示无法再访问其内的属性。但导入这个匿名包的时候，会进行一些初始化操作(例如init()函数)，如果这个初始化操作会影响当前包，那么这个匿名导入就是有意义的。

21. 变量作用域

   - 定义在函数内部的变量为局部变量，只在函数内部可见
   - 定义在代码块内(如`{...CODE...}`)的变量也是局部变量，除了代码块就消失
   - 定义在代码块外、函数外的变量为包变量或者全局变量，它们可以被同一个目录下同一个包的多个文件访问(因为Go中一个目录下只能定义一个包，但一个包可以分成多个文件)
     - 如果变量的名称以小写字母开头，则其它包不能访问该变量
     - 如果变量的名称以大写字母开头，则其它包可以访问该变量

   不同scope的变量名可以冲突，但建议采取名称唯一的方式为变量命名。

22. Integer类型当存储的是以单引号包围的字符时，它会将字符转换成它二进制值对应的数值。同样适用于unicode字符，它将用来存放各字节对应的二进制的数值。 