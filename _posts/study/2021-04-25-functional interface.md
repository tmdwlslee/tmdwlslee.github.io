---
layout: post
title:  "함수형 인터페이스란?"
date: '2021-04-25 21:51:00 +0900'
categories: study
tags: githubpage
comments: true
---

- 목차
    - [1. 함수형 인터페이스란?](#1-함수형-인터페이스란)
    - [2. 함수형 인터페이스 특징](#2-함수형-인터페이스-특징)
    - [3. 기본 함수형 인터페이스](#3-기본-함수형-인터페이스)

<br>
<br>
<br>

## 1. 함수형 인터페이스란?
---
- 단 하나의 추상 메서드만 선언된 인터페이스

```java
// @FunctionalInterface 선언 시 함수형 인터페이스로써
// 단 하나의 추상 메서드만 가져야함.
// 아닐 경우 컴파일 단계에서 Error
@FunctionalInterface
public interface MyFunctionalInterfaceTest {
    public abstract boolean cmp(int a, int b);
    //public abstract int cal(int a, int b); -> 주석 해제시 Error
}

```

<br>
<br>
<br>

## 2. 함수형 인터페이스 특징
---
- 소스의 재활용을 함수 단위로 사용 가능.
- 클래스 단위로 했을 때 보다 소스코드의 **재활용성 ↑ 하는 부분이 존재**
- 함수형 인터페이스는 기본적으로 람다식으로 간편하게 작성이 가능.
- **코드의 간결성은 증가**, 하지만 무분별하게 사용했을 시 **코드가 복잡해질 수도 있음.**

```java
public class MyFunctionalInterface {
    public static void main(String[] args) {
        // 이전에는 아래와 같이 로직을 변경할려고 했으면 아래의 절차를 진행했어야했다.
        // MyFunctionalInterfaceTest 상속 -> 클래스 내부에 cmp 메서드 구현 -> 로직 변경
        // 하지만 함수형 인터페이스로 사용하면 함수의 body를 a > b의 결과 값을 반환하도록 작성 가능
        MyFunctionalInterfaceTest func = (a,b) -> a > b;

        int a = 10;
        int b = 5;
        System.out.println("a 는 b보다 크면 -> " + func.cmp(a,b)); // true

        // 로직 변경도 함수 body 내용만 바꿔주면 된다 -> 소스코드의 재활용성 ↑
        MyFunctionalInterfaceTest func = (a,b) -> a > b;
    }
}

```

<br>
<br>
<br>

## 3. 기본 함수형 인터페이스
---
- Runnable : 매개변수를 받지않고 리턴 값도 없는 인터페이스.
- 주로 Thread 구현시 사용

```java
public interface Runnable {
    public abstract void run();
}

```

<br>

- Supplier<T> : 매개변수를 받지않고 T 타입의 객체를 리턴하는 인터페이스.
- **게으른 연산(Lazy Evaluation)**에 사용 -> **불 필요한 연산을 제거**.

```java
public interface Supplier<T> {
    T get();
}

```

<br>

```java
import java.util.concurrent.TimeUnit;

public class LazyEvaluation {
    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();

        // index가 0보다 클 때만 연산할려고 하지만 아래의 코드는 모두 3초의 시간이 경과됨.
        // Lazy Evaluation으로 해결
        printGoodNight(2, getString() +" : " +(System.currentTimeMillis() - startTime)/1000+ " sec");
        printGoodNight(1, getString() +" : " + (System.currentTimeMillis() - startTime)/1000+ " sec");
        printGoodNight(0, getString() +" : " + (System.currentTimeMillis() - startTime)/1000+ " sec");
        
        /* 출력 결과 : 
            Good night : 3 sec
            Good night : 6 sec
            No Good night : 9 sec
         */

    }

    public static String getString(){
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "Good night";
    }

    public static void printGoodNight(int index, String value){
        if(index > 0){
            System.out.println(value);
        }else{
            System.out.println("No " + value);
        }
    }
}

```
<br>

- Supplier<T>를 사용한 게으른 연산

```java
import java.util.concurrent.TimeUnit;
import java.util.function.Supplier;

public class LazyEvaluation {
    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();

        // Supplier<T>를 사용해서 연산할 때만 3초가 소요되도록 변경
        // 불 필요한 연산 수행안됨.
        lazyPrintGoodNight(2, () -> getString());
        System.out.println("index : 2 -> " + (System.currentTimeMillis() - startTime)/1000+ " sec");
        lazyPrintGoodNight(1, () -> getString());
        System.out.println("index : 1 -> " + (System.currentTimeMillis() - startTime)/1000+ " sec");
        lazyPrintGoodNight(0, () -> getString());
        System.out.println("index : 0 -> " + (System.currentTimeMillis() - startTime)/1000+ " sec");

        /* 출력 결과 :
            Good night
            index : 2 -> 3 sec
            Good night
            index : 1 -> 6 sec
            No
            index : 0 -> 6 sec
         */

    }

    public static String getString(){
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "Good night";
    }

    public static void lazyPrintGoodNight(int index, Supplier<String> func){
        // index가 0보다 클 때만 연산할려고 함
        if(index > 0){
            System.out.println(func.get());
        }else{
            System.out.println("No ");
        }
    }
}


```
<br>

- Consumer<T> : T 타입의 객체를 인자로 받고 리턴 값은 없음.
- List에 구현된 forEach 구문에서 내부적으로 사용

```java

public interface Consumer<T> {
    void accept();

    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}

```
<br>

```java
public static void main(String[] args) {
    //Consumer<T> forEach 사용
    Consumer<Integer> consumer = num -> System.out.print(num+" ");
    List<Integer> list = Arrays.asList(1,2,3,4,5);
    list.forEach(consumer);

    // 출력 결과 : 1 2 3 4 5
}
```

<br>

- Predicate<T> : T 타입의 객체를 인자로 결과로 boolean을 리턴.
- Stream의 filter 메서드에서 사용.

```java
public interface Predicate<T> {
    boolean test(T t);

    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }

    default Predicate<T> negate() {
        return (t) -> !test(t);
    }

    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }

    static <T> Predicate<T> isEqual(Object targetRef) {
        return (null == targetRef)
                ? Objects::isNull
                : object -> targetRef.equals(object);
    }
}
```

<br>

```java
public static void main(String[] args) {
    //Predicate<T>
    Predicate<Integer> predicate = num -> num < 10;
    Stream<Integer> integerStream = Stream.of(1,5,11);
    integerStream.filter(predicate).forEach(num -> System.out.print(num + " "));

    // 출력 결과 : 1 5
}

```

<br>


- Function : T타입의 인자를 받고, R타입의 객체를 리턴.
- Stream의 map 메서드에서 사용

```java
public interface Function<T, R> {
    R apply(T t);

    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }

    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

    static <T> Function<T, T> identity() {
        return t -> t;
    }
}
```
<br>

```java
public static void main(String[] args) {
    //Function<T, R>
    Function<Integer, String> function = num -> String.valueOf(num);
    Stream<Integer> funcStream = Stream.of(1,5,11);
    System.out.println(funcStream.map(function).collect(Collectors.toList()));

    //출력 결과 : [1, 5, 11]
}
```

