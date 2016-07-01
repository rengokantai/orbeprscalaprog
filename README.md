#### orbeprscalaprog
######2-4 mutable datatype
```
import scala.collection.mutable
val mutableList = mutable.ListBuffer[String]()
var immutableList = List[String]()

def appendMu(input:String):Unit={
  mutableList+=input
}
def removeMu(input:String):Unit={
  mutableList-=input
}
def appendImmu(input:String):Unit={
  immutableList:+=input
}
def removeImmu(input:String):Unit={
  immutableList = immutableList.filterNot(_==input)
}
```
######2-5 var in loop (avoid vars in for loop)
```
case class Foo(i:Int)
def myfunc3(input: List[Foo]):Int={
  var total=0
  for(i<-input){
    if(i.i%2==0){
      total=total+i.i
    }
  }
  total
}
```
invoke
```
val list=[1,2,3]
val foo = list.map(Foo)
myfunc3(foo)
```
using map:  
original:
```
def myfunc1(input: List[Int]):Int={
  input.filter(i=>i%2==0).sum
}
```
change:
```
def myfunc1(input: List[Foo]):Int={
  input.map(i=>i.i).filter(i=>i%2==0).sum
}
```
#####3
######1storing defs inside of vars
```
var f : String=>String = input=>input.toLowerCase

def myfunc(input:String):String={
  if(input.length%2==0){
    f=input=>input.toUpperCase
  }
  f(input)
}

def myfunc2(input:String):String={
  var convertor: String=>String=input=>input.toLowerCase
    if(input.length%2==0){
    convertor=input=>input.toUpperCase
  }
  convertor(input)
}
```
######2 storing functions in lists and maps
```
val strings = List("bar","foo"
)
//Not good
def filter(input:List[String]):List[String]={
  input.filterNot(_.contains("f")).filterNot(_.length%2==0)
}
def main(arg:Array[String]):Unit={
println(filter(strings))
}
```
list filters: (anoy function)
```
val filters:List[String=>Boolean]=List(
_.contains("f"),
_.length%2==0
)
def filter2(input:List[String]):List[String]={
  filters.foldLeft(input)((i,f)=>i.filterNot(f))
}
```
non-anoy function
```
def a(input:String):Boolean = input.contains("f")
def b(input:String):Boolean = input.length%2==0
var filters2:List[String=>Boolean]=List(
a,
b
)
```
######3 abstract function bodies
```
def myfunc(input:String,count:Int):List[String]={
  (for(i<- 0 until count) yield{
  if(i%2==0){
    input.toUpperCase
    }
  }).toList
}
//using map
def myfunc2(input:String,count:Int):List[String]={
  (0 until count).map(i=>
  if(i%2==0){
    input.toUpperCase
    }
  ).toList
}
def main(args:Array[String]):Unit={
  println(myfunc("hello",2))
}

```
abstract predicate:
```
def myfunc2(input:String,count:Int,predicate:Int=>Boolean i=>i%2==0):List[String]={
  (0 until count).map(i=>
  if(predicate){
    input.toUpperCase
    }
  ).toList
}
```
or using implicit predicate
```
implicit val predicate:Int=>Boolean = i=>i%2==0
def myfunc4(input:String,count:Int)(implicit predicate:Int=>Boolean):List[String]={
  (0 until count).map(i=>
  if(predicate){
    input.toUpperCase
    }
  ).toList
}
```
