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
