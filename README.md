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
#####4
######1 mutable data structures
```
import scala.collection.mutable
mutable.ListBuffer[String]().append("foo"),remove(mutableList.indexOf("foo"))
mutalbe.LinkedHashMap[Int,String]().put(1,"foo").remove(1)
```
######2 immutable data structures
list.
#####5
######1 Do not pattern match for all statements
```
def myfunc(input:Int):Boolean = input match{
  case 0|1 => true
  case _ => false
}
```
modify
```
def myfunc(input:Int):Int = input.isEmpty match{  //change here. input.isEmpty
  case true => 0
  case false => input+1
}
```
######2 cover all outcomes of a pattern match
```
def myfunc(input: Option[String]): Boolean=input match{
  case Some("foo") => false
  case Some("bar") => true
  caee Some(in) => in
  case None => "None"
}

```
######3 prefer value matches over guards
```
case class Foo(f1:Int,f2:String)
def myfunc(input:Foo):Boolean=input match{
  case Foo(f1,f2)if f1==0 && f2==""=>true    // case Foo(0,"")=>true
  case _ => false
}
```
######4 prefer to map filter over an option rather than pattern matching.
```
def myfunc(input Option[String]):Option[Int] = input match{
  case Some(in) if!in.trim.isEmpty=>Some(in.length)
  case _ => None
}
```
better:
```
def myfunc(input Option[String]):Option[Int] = {
  input.filter(_.nonEmpty).map(_.length)
}
```
######5 Prefer type matching to type casting
```
def myfinc(input:Any):String={
  input.asInstanceOf[String]  //cast to String.
}
def main(args:Array[String]):Unit={
  try{myfinc(3)}  //will throw an error
  catch{
  case e : Throwable=>a.printStackTrace()
  }
}
```
add logic
```
def myfinc(input:Any):Option[String]={
if(input.isInstanceOf[String]){
  Some(input.asInstanceOf[String])  //cast to String.
}else{
None
}
```
pattern matching
```
def myfinc(input:Any):Option[String]={
  case in: String=>Some(in)
  case _=>None
}
```
