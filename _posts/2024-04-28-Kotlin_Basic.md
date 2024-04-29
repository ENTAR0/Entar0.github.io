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

```

# 4장. Control Flow
# 5장. Functions
# 6장. Classes
# 7장. Null Safety
