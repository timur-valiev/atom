#HSLIDE
# Java
lecture 3
## Generics & Collections

#HSLIDE
### Exceptions. Anything that can go wrong will go wrong

- lost connection
- inconsistent object state
- out of memory
- file not found


#HSLIDE
### Some problems could be fixed in runtime
Recovery:
- reconnect
- fix/rebuild object
- free some memory
- create new file

or
- cancel operations


#HSLIDE
### Exceptions hierarchy
<img src="lecture03/presentation/assets/img/exception.png" alt="exception" style="width: 750px;"/>


#HSLIDE
##Checked exceptions must be handled


#HSLIDE
### Throwable
Superclass of all errors and exceptions

```java
Throwable()
Throwable(String message)
Throwable(Throwable cause)
Throwable(String message, Throwable cause)

StackTraceElement getStackTrace()
Message getMessage()
Throwable getCause()
```

#HSLIDE
### Stacktrace
```java
Exception in thread "main" java.lang.NullPointerException: Ой всё
        at ru.atom.makejar.HelloWorld.getHelloWorld(HelloWorld.java:12)
        at ru.atom.makejar.HelloWorld.main(HelloWorld.java:8)

```

Build and run jar with
```java
package ru.atom.makejar;

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println(getHelloWorld());
    }

    public static String getHelloWorld() {
        throw new NullPointerException("Ой всё");
    }
}
```


#HSLIDE
### Exception handling 
### try-catch-finally
```java
try {
    statements
} catch (ExceptionType1 e) {
    statements
// more catches    
} catch (ExceptionType2 e) {
    statements
} finally {
    statements
}
```


#HSLIDE
### Exception handling example
```java
try {
    String possibleInt = "itsNotANumber";
    return Integer.parseInt(possibleInt);
} catch (ExceptionType e) {
    log.error("Exception in possibleInt parsing. {} is not an integer", possibleInt);
    return DEFAULT_INT_VALUE; 
} finally {
    if (dbConnection != null 
        && dbConnection.isOpened()) {
        
        dbConnection.close();
    }
}
```


#HSLIDE
### Exceptions
@See ru.atom.exception

1. try-catch-finally
1. catch or throws
1. multiple catches
1. try with resources


#HSLIDE
### Generics
1. External libraries usage
1. Exceptions
1. **[Generics]**
1. Collections
1. Homework 2

#HSLIDE
### non-generic

```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

#HSLIDE
### Problem
In our code
```java
Box box = new Box();
String content = "Awesome content";
box.set(content);
```

In a galaxy far far away
```java
Object content = box.getContent();
// Nooooooooooooooo
// cast is not type safe
```

      
#HSLIDE
### Generic Solution
Definition
```java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

Usage
```java
Box<String> box = new Box<>();
String content = "Awesome content";
box.set(content);

String outgoing = box.getContent();

assertThat(outgoint, is(equalTo(content))); // <-- OK
```
      
      
#HSLIDE
### Generics in Java 

1. Way to implement general purpose algorithms
1. Way to avoid type casting
1. Provide type casting under the hood
1. Present and processed in **Compile-time**. Absent in runtime*       
      

#HSLIDE
### Generics Definition
Generic class
```java
class Clazz<T1, T2, T3> {
}

Clazz<Boolean, Integer, String> instance = new Clazz<>();
```

Generic method
```java
class Clazz {
    public <T> void foo(T t) {
        log.info("Generic method call with param {}", t);
    }
}

Clazz instance = new Clazz();
instance.foo("string value");
```
[Read more in official documentation](https://docs.oracle.com/javase/tutorial/java/generics/methods.html)


#HSLIDE
### ArrayList. Summary #1
- Auto resizable-array implementation of the `List` interface. i.e. dynamic array
- Place in hierarchy:
```java
    java.lang.Object
        java.util.AbstractCollection<E>
            java.util.AbstractList<E>
                java.util.ArrayList<E>
```

#HSLIDE
### ArrayList. Summary #2
- Providing interfaces
    - List 
    - RandomAccess
    - Serializable 
    - Cloneable
    - Collection
    - Iterable

[Read more (RU)](https://habrahabr.ru/post/128269/)


#HSLIDE
### ArrayList is a List
List is a Ordered Collection or sequence
```java
interface List<E> extends Collection<E> {
    E get(int index);
    E set(int index, E element);
    void add(int index, E element);
    E remove(int index);
    List<E> subList(int fromIndex, int toIndex);
    // ...
}
```


#HSLIDE
### ArrayList. Internals #1

```java
List<String> list = new ArrayList<>();
```
<img src="lecture03/presentation/assets/img/newarray.png" alt="exception" style="width: 600px;"/>

```java
list.add("0");
list.add("1");
```

<img src="lecture03/presentation/assets/img/array1.png" alt="exception" style="width: 600px;"/>


#HSLIDE
### ArrayList. Internals #2
```java
list.addAll(Arrays.asList("2", "3", "4", "5", "6", "7", "8"));
list.add("9");
```

<img src="lecture03/presentation/assets/img/array9.png" alt="exception" style="width: 600px;"/>


#HSLIDE
### ArrayList. Internals #3
```java
list.add("10");
```
Not enough capacity. Need (auto)resize.

<img src="lecture03/presentation/assets/img/arrayresized.png" alt="exception" style="width: 750px;"/>

<img src="lecture03/presentation/assets/img/array10.png" alt="exception" style="width: 750px;"/>

#HSLIDE
### ArrayList. Complexity

|  contains  | add   | get   |  set  | remove | 
|:----------:|:-----:|:-----:|:-----:|:------:|
| O(n)       | O(1)* |  O(1) |  O(1) | O(n)   |


#HSLIDE
### ArrayList is a RandomAccess List

RandomAccess - marker interface.
Indicate that List support fast (generally constant time) random access.

RandomAccess helps generics algorithms.


#HSLIDE
### List is a Collection

Collection is a root interface of Collection Framework.

Collection represents a group of objects.

```java
interface Collection<E> extends Iterable<E> {
    int size();
    
    boolean contains(Object o);
    boolean add(E e);
    boolean remove(Object o);
    Iterator<E> iterator();
}
```


#HSLIDE
### Collection is an Iterable
```java
interface Iterable<T> {
    Iterator<T> iterator();
    default void forEach(Consumer<? super T> action) { ... }
}

interface Iterator<E> {
    boolean hasNext();
    E next();
}
```


#HSLIDE
### Single interface - multiple implementations #1
<img src="lecture03/presentation/assets/img/collectionimpl.png" alt="exception" style="width: 750px;"/>


#HSLIDE
### Single interface - multiple implementations #2
List implementations:
- ArrayList
- CopyOnWriteArrayList - thread-safe variant of ArrayList
- LinkedList
- Vector - synchronized list
- ...


#HSLIDE
### CopyOnWriteArrayList
A **thread-safe** variant of ArrayList in which all mutative
operations (**add**, **set**, and so on) are implemented by
making a fresh copy of the underlying array.

Complexity

|  contains  | add   | get   |  set  | remove | 
|:----------:|:-----:|:-----:|:-----:|:------:|
| O(n)       | O(n)  |  O(1) |  O(n) | O(n)   |


#HSLIDE
### LinkedList. Summary #1
- Doubly-linked list implementation of the **List** and **Deque**
interfaces.

- Place in hierarchy:
```java
java.lang.Object
    java.util.AbstractCollection<E>
        java.util.AbstractList<E>
            java.util.AbstractSequentialList<E>
                java.util.LinkedList<E>
```


#HSLIDE
### LinkedList. Summary #2
- Providing interfaces
    - List
    - Deque
    - Queue
    - Serializable 
    - Cloneable
    - Iterable 
    - Collection


#HSLIDE
### LinkedList internals #1
```java
List<String> list = new LinkedList<>();
```

<img src="lecture03/presentation/assets/img/linkednew.png" alt="exception" style="width: 400px;"/>


#HSLIDE
### LinkedList internals #2
```java
list.add("0");
```

Allocation

<img src="lecture03/presentation/assets/img/linked0.png" alt="exception" style="width: 400px;"/>

Linking

<img src="lecture03/presentation/assets/img/linked0linked.png" alt="exception" style="width: 400px;"/>


#HSLIDE
### LinkedList internals #3
```java
list.add("1");
```

Allocation

<img src="lecture03/presentation/assets/img/linked1.png" alt="exception" style="width: 400px;"/>

Linking

<img src="lecture03/presentation/assets/img/linked1linked.png" alt="exception" style="width: 400px;"/>


#HSLIDE
### LinkedList. Complexity

|  contains  | add   | get   |  set  | remove | 
|:----------:|:-----:|:-----:|:-----:|:------:|
| O(n)       | O(1)  |  O(n) |  O(n) | O(n)   |


#HSLIDE
### Practice
@See ru.atom.list

1. Implement ListNode class
1. Implement absent methods in CustomLinkedList
1. Implement Iterator for CustomLinkedList
1. Remove @Ignore in CustomLinkedListTest
1. All CustomLinkedListTest tests should pass

3 Points


#HSLIDE
### Interface Set
-  A collection that contains no duplicate elements.  More formally, sets
   contain no pair of elements **e1** and **e2** such that
    ```java
       e1.equals(e2);
    ```
    
- Implementations
    - HashSet 
    - TreeSet
    - EnumSet 
    - ConcurrentSkipListSet 
    - CopyOnWriteArraySet 
    - LinkedHashSet
    - ...


#HSLIDE
### HashSet
Set interface implementation, backed by a hash table (actually a HashMap instance).
It makes no guarantees as to the iteration order of the set.


#HSLIDE
### HashSet. Internals
<img src="lecture03/presentation/assets/img/hashset.png" alt="exception" style="width: 600px;"/> 
 

#HSLIDE
### General contract
For objects **a** and **b**:
```java
a.equals(b) => a.hashCode() == b.hashCode()

if a.hashCode() == b.hashCode() 
          a may be not equal b
          
a.hashcode() is the same during object lifetime
```


#HSLIDE
### TreeSet
The elements are ordered using their **Comparable** natural 
ordering, or by **Comparator** provided at set creation time, 
depending on which constructor is used.


#HSLIDE
### TreeSet. Internals
<img src="lecture03/presentation/assets/img/treeset.png" alt="exception" style="width: 600px;"/>

[Read more (RU)](https://habrahabr.ru/post/65617/)

#HSLIDE
### Functional interface Comparable<T>

```java
@Override
public int compareTo(T o) {
	return this.field – o.field;
}
```


#HSLIDE
### Map 

```java
interface Map<K, V>
``` 

An object that maps keys to values.
Cannot contain duplicate keys.
Each key map to at most one value.
 

#HSLIDE
### Map methods
 ```java
boolean containsKey(Object key);
V get(Object key);
V put(K key, V value);
V remove(Object key);
```

#HSLIDE
### Why, Map?
Why Map is not a Collection?

#HSLIDE
### Why, Map?
From official FAQ:
> This was by design.
> We feel that mappings are not collections and collections are not mappings. 
> If a Map is a Collection, what are the elements? 


#HSLIDE
### Map implementations
- HashMap
- TreeMap
- LinkedHashMap
- EnumMap


#HSLIDE
### Notes
1. HashSet is cutted HashMap
1. TreeSet is cutted TreeMap 

[Read more](http://infotechgems.blogspot.ru/2011/11/java-collections-performance-time.html)


#HSLIDE
### HashMap. Internals 
<img src="lecture03/presentation/assets/img/hashmap.png" alt="exception" style="width: 750px;"/>

#HSLIDE
### Java8. Streams & Lambdas
How it was before?
```java
List<Person> people = Arrays.asList(new Person("a", 2), new Person("b", 3));

List<Integer> ages = new ArrayList<Integer>();
       
for (Person p : people) {
    ages.add(p.getAge());
}
```
Now
```java
List<Integer> ages2 = people.stream().map(Person::getAge).collect(Collectors.toList());
```
Or
```java
List<Integer> ages2 = people.stream().map(p->p.getAge()).collect(Collectors.toList());
```

#HSLIDE
### Maven
Project build and dependencies management.

#HSLIDE
### Spring Framework

1. Dependency Injection
2. Aspect-Oriented Programming including Spring's declarative transaction management
3. Spring MVC and Spring WebFlux web frameworks
4. Foundational support for JDBC, JPA, JMS
5. Much more…

#HSLIDE
### Spring Boot

1. A framework to manage framework
2. Starters

<img src="http://i0.kym-cdn.com/photos/images/facebook/000/531/557/a88.jpg" alt="exception" style="width: 400px;"/>


