---
title: "Kotlin 기초"
date: 2024-04-25 02:23:00 +0900
categories: [Kotlin]
tags: [Language, Java, Spring, Kotlin, Back-end]
---
# 1장. Hello, World

```kotlin
fun main() {
    println("Hello, World!")
}
```

- <code clas="code ">fun</code>은 Function 선언에 사용됩니다.
- <code clas="code ">main()</code> function은 플랫폼이 시작되는 곳입니다.
- Function의 Body는 중괄호 <code clas="code ">{}</code>안에 쓰여집니다.
- <code clas="code ">println</code>과 <code clas="code ">print()</code> function은 arguments를 출력하는 Standard Output function입니다.

### Variables
코틀린에서는 변수를 선언할때 자료형을 제외하고 선언해줘야하는게 하나 더 있습니다.
- read-only varables with <code clas="code ">val</code>
- mutable variables with <code clas="code ">var</code>

값을 할당할때는 지정연산자 <code clas="code ">=</code> 사용합니다.

```kotlin
val papcorn = 5
val hotdog = 7
var customers = 10

hotdog = 6 // X
customers = 8 // O
```

* Kotlin에서는 기본적으로 read-only 변수인 val을 사용하기를 권장합니다. <code clas="code ">"</code>를 이용해서 묶고, 변수는 <code clas="code ">$</code>뒤에 적어 출력합니다.

```kotlin
val customers = 10
println("There are $customers customers")
// There are 10 customers
println("There are ${customers + 1} customers")
// There are 11 customers
```

### String Templates
변수를 표준 출력하기 위해서 사용하는 string templates에 대해 소개합니다. 문자열 값은 


# 2장. Basic Types
코틀린은 기본적으로 tpye inference 기능을 가지고있어 타입을 명시하지 않더라도 컴파일러가 스스로 해당 변수에 선언되어있는 값을 참고해 무슨 타입인지 정합니다.
```kotlin
var customers = 10

customers = 8

customers = customers + 3
customers += 7
customers -= 3
customers *= 2
customers /= 3

println(customers)
```

|Category|Basic types|
|-------------|--------------|
|Intergers|<code clas="code ">Byte</code>, <code clas="code ">Short</code>, <code clas="code ">Int</code>, <code clas="code ">Long</code>|
|Unsigned integers|<code clas="code ">UByte</code>, <code clas="code ">UShort</code>, <code clas="code ">UInt</code>, <code clas="code ">ULong</code>|
|Floating-pint numbers|<code clas="code ">Float</code>, <code clas="code ">Double</code>|
|Booleans|<code clas="code ">Boolean</code>|
|Characters|<code clas="code ">Char</code>|
|String|<code clas="code ">String</code>|

##### For more information about Basic Types : https://kotlinlang.org/docs/basic-types.html

```kotlin
//Variable declared without init
val d: Int
//Variable inited
d = 3
//Variable explicitly typed and initialized
val e: String = "hello"

println(d)
println(e)
```

# 3장. Collections
코틀린에서도 리스트, 딕셔너리, 집합등의 여러 Collection Data Type을 지원해준다.

|Collection type|Description|
|---------------|-----------|
|Lists| Ordered Collection of items|
|Sets|Unique unordered collections of items|
|Maps|Sets of key-value paris where keys are unique and map to only one value|

각각의 Collection Type은 mutable 또는 read-only로 선언할 수 있다.

### List

- To create a read-only list (<code clas="code ">List</code>), use the <code clas="code ">listOf</code> function.

- To create a mutable list (<code clas="code ">MutableList</code>), use the <code clas="code ">mutableListOf</code> function.

* 기본적으로 List 선언시 컴파일러가 Type of item을 참고해 정해주지만, List 선언 후 <code clas="code "><></code>안에 명시해줄 수 있습니다.

```kotlin
//Read-only List
val readOnlyShapes = listOf("triangle", "square", "circle")
println(readOnlyShapes)

//Mutable list with explicit type declaration
val shapes: MutableList<String> = mutableListOf("triangle","square", "circle")
println(shapes)
```

캐스팅을 사용하여 원본 데이터의 원치않는 수정을 막을 수 있습니다.
```kotlin
val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")
val shapesLocked: List<String> = shapes
```

List item에 접근할때는 인덱스 접근 연산자<code clas="code ">[]</code>를 사용합니다.

```kotlin
val readOnlyShapes = listOf("trianlge", "square", "circle")
println("The first item in the list is: ${readOnlyShapes[0]}")
```

또는 <code clas="code ">.first()</code>와 <code clas="code ">.last()</code> functions을 활용할 수 도 있습니다.

```kotlin
val readOnlyShapes = listOf("triangle", "square", "circle")
println("The first item in the list is ${readOnlyShapes.first()}")
```

* Python의 method라 불리는 기능을 Kotlin에서는 extension function이라고 지칭합니다. 똑같이 Object에 .을 붙인뒤 사용합니다.

List item 갯수에 대한 extension function

```kotlin
val readOnlyShapes = listOf("triangle", "square", "circle")
println("This list has ${readOnlyShapes.count()} items")
```

List안에 해당 item이 존재하는지 확인하는 연산자 <code clas="code ">in</code>

```kotlin
val readOnlyShapes = listOf("triangle", "square", "circle")
println("circle" in readOnlyShapes)
//true
```

mutable List에 <code clas="code ">.add()</code>와 <code clas="code ">.remove()</code> function 사용

```kotlin
val shapes: MutableList<String> = mutableListOf("trianlge", "square", "circle")
shapes.add("pentagon")
println(shapes)

shapes.remove("triangle")
println(shapes)
```

### Set

Set은 List와 다르게 Unordered하고 Unique한 Item을 담는 특성을 가지고 있습니다.

- To create a read-only set(<code clas="code ">Set</code>), use the <code clas="code ">setOf()</code> function.

- To create a mutable set (<code clas="code ">MutableSet</code>), use the <code clas="code ">mutableSetOf()</code> function.

List와 마찬가지로 set도 Type inference와 <code clas="code "><></code>안에 타입 명시가 가능합니다.

```kotlin
//Read-only set
val readOnlyFruit = setOf("apple", "banana", "cherry", "cherry")
//Mutable set wit explicit type declaration
val fruit: MutableSet<String> = mutableSetOf("apple", "banana","cherry", "cherry")

println(readOnlyFruit)
// [apple, banana, cherry]
```

set도 캐스팅이 가능합니다.

* Set은 Unordered 특성을 가져 특정 인덱스 접근 방법은 불가능합니다.

Set안에 아이템 갯수를 세기위한 <code clas="code ">.count()</code> function

```Kotlin
val readOnlyFruit = setOf("apple", "banana", "cherry", "cherry")
println("This is set has ${readOnlyFruit.count()} items")
```

set안에 item이 존재하는지 확인할때는 <code clas="code ">in</code>연산자를 사용합니다.

```Kotlin
val readOnlyFruit = setOf("apple", "banana", "cherry", "cherry")
println("banana" in readOnlyFruit)
//true
```

items을 mutable set에 추가하거나 삭제할때는 <code clas="code ">.add()</code>와 <code clas="code ">.remove()</code> extension을 사용합니다.

val fruit: MutableSet<String> = mutableSetOf("apple", "banana", "cherry", "cherry")
fruit.add("dragonfruit")
println(fruit)

fruit.remove("dragonfruit")
println(fruit)

### Map

Map은 dictionary 타입과 같이 key-value정보를 쌍으로 갖고 있습니다. key를 참조해서 value에 접근할 수 있습니다.

* Kotlin에서는 key가 unique한 특성을 가지고 value는 아무거나 지정가능합니다.
* Map 자료형안에서 value를 복제하는것도 가능합니다.

- To create a read-only map(<code clas="code ">Map</code>), use the <code clas="code ">mapOf()</code> function.

- To create a mutable map(<code clas="code ">MutableMap</code>), use the <code clas="code ">mutableMapOf</code> function.

Kotlin에서는 item을 만들때 컴파일러가 항목의 타입을 추정해서 저장합니다. 타입을 명시하기 위해서는 key와 value 값을 <code clas="code "><></code>안에 명시해주면 됩니다.

```Kotlin
val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
println(readOnlyJuiceMenu)
//{apple=100, kiwi=190, orange=100}

//Mutable map with explicit type declaration
val juiceMenu: MutableMap<String, Int> = mutableMapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
println(juiceMenu)
```

* Map도 캐스팅 가능합니다.

value에 접근하기 위해서는 <code clas="code ">[]</code>안에 쌍을 이루는 key를 적어 접근합니다.

```Kotlin
val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
println("The value of apple juice is : ${readOnlyJuiceMenu["apple"]})
// The value of apple juice is : 100
```

items을 mutable map에 추가하거나 삭제할때는 <code clas="code ">.put()</code>와 <code clas="code ">.remove()</code> extension을 사용합니다.

```Kotlin
val juiceMenu: MutableMap<String, Int> = mutableMapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
juiceMenu.put("coconut", 150)
println(juiceMenu)
// {apple=100, kiwi=190, orange=100, coconut=150}

juiceMenu.remove("orange")
println(juiceMenu)
// {apple=100, kiwi=190, coconut=150}
```

map안에 이미 key가 존재하는지 확인할 때는 <code clas="code ">.containskey()</code> extension를 사용합니다.
```Kotlin
val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
println(readOnlyJuiceMenu.containsKey("kiwi"))
//true
```

key와 value를 collection으로 추출할때는 <code clas="code ">keys</code>와 <code clas="code ">values</code> properties를 사용합니다.

```Kotlin
val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
println(readOnlyJuiceMenu.keys)
// [apple, kiwi, orange]
println(readOnlyJuiceMenu.values)
// [100, 190, 100]
```

map안에 key와 value에 대해 확인하고 싶을 때는 <code clas="code ">in</code>연산자 사용합니다.

```Kotlin
val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
println("orange" in readOnlyJuiceMenu.keys)
//true
println(200 in readOnlyJuiceMenu.values)
//false
```

# 4장. Control Flow

### Conditional expressions

#### if

```
val d: Int
val check = true

if(check) {
    d = 1
}
else {
    d = 2
}

println(d)
```

Kotlin에서는 ternary operator(<code clas="code ">condition ? then : else</code>)가 없는 대신 다른 표현법이 있습니다.

```
val a = 1
val b = 2

println(if(a > b) a else b) //Returns a value : 2
```

#### When

switch-case문과 비슷합니다.

```Kotlin
val obj = "Hello"

when (obj) {
    //Checks whether obj equals to "1"
    "1" -> println("One")
    //Checks whether obj equals to "Hello"
    "Hello" -> println("Greeting")
    //Default statement
    else -> println("Unknown")
}
```

또한 <code clas="code ">When</code>문법은 변수에 할당이 가능합니다.

```Kotlin
val obj = "Hello"    

val result = when (obj) {
    // If obj equals "1", sets result to "one"
    "1" -> "One"
    // If obj equals "Hello", sets result to "Greeting"
    "Hello" -> "Greeting"
    // Sets result to "Unknown" if no previous condition is satisfied
    else -> "Unknown"
}
println(result)
// Greeting
```

또한 <code clas="code ">When</code>문법은 불 표현식과 연계해서도 사용 가능합니다.

```Kotlin
val temp = 18

val description = when {
    // If temp < 0 is true, sets description to "very cold"
    temp < 0 -> "very cold"
    // If temp < 10 is true, sets description to "a bit cold"
    temp < 10 -> "a bit cold"
    // If temp < 20 is true, sets description to "warm"
    temp < 20 -> "warm"
    // Sets description to "hot" if no previous condition is satisfied
    else -> "hot"             
}
println(description)
// warm
```

### Ranges

4가지 표현식이 존재합니다.

1. <code clas="code ">..</code> Operator : <code clas="code ">1..4</code> == <code clas="code ">1, 2, 3, 4</code>
2. <code clas="code ">..<</code> Operator : <code clas="code ">1..<4</code> == <code clas="code ">1, 2, 3</code>
3. <code clas="code ">dwonTo</code> : <code clas="code ">4 downTo 1</code> == <code clas="code ">4, 3, 2, 1</code>
4. <code clas="code ">step</code> : <code clas="code ">1..5 step 2</code> == <code clas="code ">1, 3, 5</code>

<code clas="code ">Char</code>도 ranges 가능합니다.

- <code clas="code ">'a'..'d'</code> == <code clas="code ">'a', 'b', 'c', 'd'</code>
- <code clas="code ">'z' downTo 's' step 2</code> == <code clas="code ">'z', 'x', 'v', 't'</code>

### Loops

루프 중 가장 흔하게 사용하는것은 <code clas="code ">for</code>와 <code clas="code ">While</code>입니다.

#### For

```Kotlin
for (number in 1..5) {
    printf(number)
}
//12345
```

```Kotlin
//Collections
val cakes = listOf("carrot", "cheese", "chocolate")

for (cake in cakes) {
    println("Yummy, it's $cake cake!")
}
// Yummy, it's a carrot cake!
// Yummy, it's a cheese cake!
// Yummy, it's a chocolate cake!
```

#### while
<code clas="code ">while</code>은 두가지 방법으로 쓰입니다.

- To execute a code block while a conditional expression is true. (<code clas="code ">while</code>)

- To execute the code block first and then check the conditional expression. (<code clas="code ">do-while</code>)

```Kotlin
var cakesEaten = 0
while (cakesEaten < 3) {
    println("Eat a cake")
    cakesEaten++
}
// Eat a cake
// Eat a cake
// Eat a cake
```

```Kotlin
var cakesEaten = 0
var cakesBaked = 0
while (cakesEaten < 3) {
    println("Eat a cake")
    cakesEaten++
}
do {
    println("Bake a cake")
    cakesBaked++
} while (cakesBaked < cakesEaten)
// Eat a cake
// Eat a cake
// Eat a cake
// Bake a cake
// Bake a cake
// Bake a cake
```

# 5장. Functions
# 6장. Classes
# 7장. Null Safety
