#HSLIDE
# Java

#HSLIDE
## Common Java facts
- Java is **crossplatform** - 'Write Once, Run Anywhere' **(WORA)**
- Java is compiled to **Byte Code** (not to machine codes), which is executed by **Java Virtual Machine (JVM)**
- automatic memory management **(GC)**
- Java is **object-oriented**, **class-based**
- static strong safe typization
- concurrent

#HSLIDE 
## Basic types
| Type          | Size          | Range             |
| ------------- |:-------------:| -----------------:|
| boolean       | undefined*    | true/false        |
| byte          | 1 byte        | -128-127          |
| char          | 2 bytes       | \u0000-\uffff     |
| short         | 2 bytes       | -32768 - 32767    |
| int           | 4 bytes       | -2^31 - (2^31)-1  |
| long          | 8 bytes       | -2^63 - (2^63)-1  |
| float         | 4 bytes       | IEEE 754          |
| double        | 8 bytes       | IEEE 754          |
| **reference** | system bitness|                   |
* Not defined by specification, but actually 1 byte in hotspot.

#HSLIDE 
## Operators
|Operator type  |Operator                   |
|---------------|---------------------------|
|Assignment     | =, +=, *= …^=             |
|Arithmetical   | +, -, *, /, %             |
|Relational     | <, >, <=, >=, ==, !=      |
|Logical        | &&, &#124;&#124;          |
|Bitwise        | &, &#124;, ^, >>, <<, >>> |
|Unary          | ++, --, +, -, !           |
|Relational2    | instanceof                |


#HSLIDE
# Expressions

```java
int value = 0;

int[] array = new int[10];
array[0] = 100;

System.out.println("Hello, world!");

if (value1 == value2)
    System.out.println("value1 == value2");

int commonVariable = 0;
if (commonVariable > -42) { // ← начало блока
    int innerVariable = commonVariable + 1;
    System.out.println(String.format(“Inner variable is %d“, innerVariable));
} // ← конец блока
/*
    а здесь innerVariable уже нет
*/
```
#HSLIDE
## if else
```java
if (18 == yourAge) {
    // у вас всё хорошо
} else if (yourAge > 18
           && yourAge <= 25) {
    // бывало и лучше
} else {
    // ¯\_(ツ)_/¯
}
```
#HSLIDE
## switch case
```java
switch (countOfApple) {
    case 1: // у нас есть 1 яблоко
        break;
    case 2: // у нас есть 2 яблока
        break;
    …
    default: 
        // прочие случаи
        break;
}

```

#HSLIDE
## loops
```
while (expression) statement

do { statement } while (expression)

for (initialization; termination; increment)
    statement
```
**Examples:**
```java
int[] digits = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
for (int i : digits) {
    System.out.println(“Digit: “ + i);
}

//Итерация для хипстеров
IntStream.range(0, 10).forEach(digit -> System.out.println(digit));

IntStream.range(0, 10).forEach(System.out::println);
```

#HSLIDE 
# Methods
```java
public int getCountOfApples(List<Integer> boxes, Integer[] numberOfBoxes) 
        throws Throwable {

    Integer sumOfApples = 0;
    for (Integer i : numberOfBoxes) {
        sumOfApples += boxes.get(i);
    }
    return sumOfApples;
}
```
**Method signature** – method name + argument list

Access modifier **public**  
Return type **int**  
Method name **getCountOfApples**  
Parameter list **( … )**  
Exception list **throws Throwable**  
Method body **{ … }**  

#HSLIDE
## JDK/JRE/JVM
**JDK** - Java Development Kit  
**JRE** - Java Runtime Environment  
**JVM** - Java Virtual Machine 
<img src="lecture01/presentation/assets/img/jdk-jre2.png" alt="me" style="width: 900px; float: left;"/>  

#HSLIDE
**JDK** - Java Development Kit  
**JRE** - Java Runtime Environment  
**JVM** - Java Virtual Machine   
<img src="lecture01/presentation/assets/img/jdk-jre.png" alt="me" style="width: 750px; float: left;"/>  
  
**JDK** =  
JRE + Tools  
  
**JRE** =  
JVM + Lang + Libs

#HSLIDE 
## From source to running program
<img src="lecture01/presentation/assets/img/codeflow.png" alt="me" style="width: 750px; float: left;"/>  

#HSLIDE 
## JDK setup *nix 
set **path** and **JAVA_HOME** environment variables  
**Linux:**
```bash
> echo "PATH='/path/to/jdk9/bin:$PATH'" >> ~/.bashrc
> echo "JAVA_HOME='/path/to/jdk9/'" >> ~/.bashrc
> source ~/.bashrc
> echo $PATH
...
> java -version
...
```
**macOS:** (possibly sudo)
```bash
> echo "PATH='/path/to/jdk9/bin:$PATH'" >> /etc/profile
> echo "JAVA_HOME='/path/to/jdk9/'" >> /etc/profile
> source /etc/profile
> echo $PATH
...
> java -version
...
```

#HSLIDE
# First program
All the code in contained in **.java** files.  
Let's create our first java program in file with name **HelloWorld.java**  

#HSLIDE 
# Hello, World!
**HelloWorld.java**
```java
public class HelloWorld {
    public static void main(String[] args) { //entry point
        System.out.println("Hello, World!");
    }
}
```
1. ```public static void main(String[] args)``` is an entry point
2. All executable code must be inside **classes**
3. public class name **must** match file name
4. ```System.out.println("Hello, World!");``` - is a standard way to print to console
5. Every statement must end with **;**

#HSLIDE 
# Compile and run

1. Compile program with **javac**
```bash
> javac HelloWorld.java
```

2. Run program with **java**
```bash
> java HelloWorld
Hello, World!
```
#HSLIDE
## Java distribution
Multiple **.class** files are not handy for distribution
(what if our project is big and we want to use a number of libraries)
  
So they use java archives (**jar**) that contain all necessary class files and custom content
