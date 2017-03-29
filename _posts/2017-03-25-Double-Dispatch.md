---
layout: post
title: Double Dispatch
category: Java
date:   2017-03-28
tags:  Java DoubleDispatch
author: SiHun
---

* content
{:toc}

Double Dispatch
===============
[토비의 봄 TV 1회 - 재사용성과 다이나믹 디스패치, 더블 디스패치](https://www.youtube.com/watch?v=s-tXAHub6vg&t=85s)
# Dispatch
## 의미
### 사전적 의미
1. (특히 특별한 목적을 위해) 보내다   
2. (편지・소포・메시지를) 보내다   
3. 신속히 해...

#### 프로그램에서의 의미
프로그램이 어떤 메소드를 호출할 것인가를 결정하여 그것을 실행하는 과정을 말한다.

# 정적인 디스패치 (Static Dispatch)
```java
public class Dispatch{
 
  static class Service{
    void run(){
      System.out.println("run");
    }
   
    void run(String msg){
      System.out.println(msg);
    }
  }
 
  public static void main(String[] args){
    new Service().run();
  }
  
}
```
위 프로그램에서 main을 실행하게 되면 Service 클래스에 정의된 run 메소드 중 어느 것이 실행된다는 것은 런타임 시점이 아닌 컴파일 시점에서 알 수 있다.
# 동적인 디스패치 (Dynamic Dispatch)
>## 코드 이해 를 위한 사전 지식
>### MethodSignature
>Quoting from Oracle Docs:
>Definition: Two of the components of a method declaration comprise the method signature—the method's name and the parameter types.
>![](https://i.stack.imgur.com/NLlbQ.png)
>자바에서 MethodSignature 는 메소드 이름 + 파라미터 이다. 반환값(return)은 포함하지 않는다.
```java
public class Dispatch{
 
  static abstract class Service{
    abstract void run();
  }
 
  static class MyService1 extends Service{
    @Overried
    void run(){
      System.out.println("1");
    }
  }
 
  static class MyService2 extends Service{
    @Overried
    void run(){
      System.out.println("2");
    }
  }
 
  public static void main(String[] args){
    MyService svc = new MyService1();
    svc.run();
  }
 
}
```
위 프로그램에서 main을 실행하게 되면 어떤 클래스의 run 메소드가 실행될지는 컴파일 시점에 알 수 없다.그러나 실제 실행을 해보면 "1"이 출력될 것임은 자명하다. 이때 다이나믹 디스패치가 일어난다.

출처: http://feco.tistory.com/86 [wmJun]

# 더블 디스패치 (Double Dispatch)

>## 코드 이해 를 위한 사전 지식
>[1.Lambda Expressions](http://jdm.kr/blog/181)
>
>[2.Method Reference](http://multifrontgarden.tistory.com/126)

페이스북, 트위터에 사진과 텍스트를 올려주는 그런 요구사항이 들어왔다고 가정하자. 그래서 아래와 같이 만들었다.
```java
interface Post {
  void postOn(SNS s);
}

static class Text implements Post {
  @Override
  public void postOn(SNS s) {
    System.out.println("text - " + s.getClass().getSimpleName());
  }
}

static class Picture implements Post {
  @Override
  public void postOn(SNS s) {
    System.out.println("picture - " + s.getClass().getSimpleName());
  }
}

interface SNS {
}

static class Facebook implements SNS {
  //...
}

static class Twitter implements SNS {
  //...
}

public static void main(String[] args) {
  List<Post> posts = Arrays.asList(new Text(), new Picture());
  List<SNS> sns = Arrays.asList(new Facebook(), new Twitter());
  posts.forEach(p -> sns.forEach(s -> p.postOn(s)));
}
```
나름 확장성을 고려하여 Post와 SNS를 인터페이스로 만들고 Post는 SNS 인터페이스를 의존하게 만들었다. 출력 결과는 다음과 같다.

text - Facebook
text - Twitter
picture - Facebook
picture - Twitter
아주 맘에 든다. 그런데 가만보면 위의 코드는 아주 간단한 코드이다. 각각의 클래스명만 가져오는 동일한 코드가 들어 있다. 물론 동일한 로직이 들어있을 수도 있겠지만 보통은 각각 다른 비지니스를 정해 줄 때도 있다. 그래서 페이스북에 올릴 때와 트위터에 올릴때 각각 다른 비지니스를 넣어주려고 한다. 그래서 아래와 같이 바꾸었다.

//나머지는 동일 해서 생략
```java
static class Text implements Post {
  @Override
  public void postOn(SNS s) {
    if (s instanceof Facebook) {
      System.out.println("text - facebook");
    }
    if(s instanceof Twitter){
      System.out.println("text - twitter");
    }
  }
}

static class Picture implements Post {
  @Override
  public void postOn(SNS s) {
    if (s instanceof Facebook) {
      System.out.println("picture - facebook");
    }
    if(s instanceof Twitter){
      System.out.println("picture - twitter");
    }
  }
}
```
 
변경된 부분만 살펴보자. 우리는 postOn안에 instanceof를 사용해서 Facebook일 때는 Facebook 비지니스로직, Twitter일때는 Twitter의 비지니스로직으로 변경하였다. 그렇게 썩 마음에 드는 코드는 아니지만 코드를 돌려보면 우리가 원하는 결과는 나온다. 그런데 갑자기 다른 SNS가 추가 되었다. Linkedin 이라는 SNS가 추가 되어 다시 개발하게 되었다.
그래서 다음과 같이 추가 하였다.
```java
static class Linkedin implements SNS {
}
static class Text implements Post {
  @Override
  public void postOn(SNS s) {
    if (s instanceof Facebook) {
      System.out.println("text - facebook");
    }
    if(s instanceof Twitter){
      System.out.println("text - twitter");
    }
    if(s instanceof Linkedin){
      System.out.println("text - linkedin");
    }
  }
}

static class Picture implements Post {
  @Override
  public void postOn(SNS s) {
    if (s instanceof Facebook) {
      System.out.println("picture - facebook");
    }
    if(s instanceof Twitter){
      System.out.println("picture - twitter");
    }
  }
}
```
정상작동 할 것처럼 보이지만 실수로 우리는 Picture에 Linkedin을 만들지 않았다. 물론 간단하니까 그냥 한눈에 보이지만 어마어마하게 많은 로직이 숨어 있다면 찾기도 어려울지도 모른다. 또한 또다른 SNS가 추가 될 때 마다 맘에 안드는 if문 계속 추가 해야되는 단점이 숨어 있다. 물론 그렇게 해도 상관은 없다. 하지만 우리는 좀 더 나은 방법을 원한다. 그래서 아래와 같이 변경을 하였다.
```java
interface Post {
  void postOn(Facebook facebook);
  void postOn(Twitter twitter);
}

static class Text implements Post {

  @Override
  public void postOn(Twitter twitter) {
    System.out.println("text - facebook");
  }

  @Override
  public void postOn(Facebook facebook) {
    System.out.println("text - twitter");
  }
}

static class Picture implements Post {

  @Override
  public void postOn(Twitter twitter) {
    System.out.println("picture - facebook");
  }

  @Override
  public void postOn(Facebook facebook) {
    System.out.println("picture - twitter");
  }
}

interface SNS {
}

static class Facebook implements SNS {
}

static class Twitter implements SNS {
}
```

Post를 SNS를 의존하는게 아니라 구현체인 Facebook과 Twitter를 의존하고 있다. 그렇게 나쁘지 않은 방식이다. 하지만 여기에서도 다른 SNS 추가 되면 Post 인터페이스를 수정해야 하고 그에 따른 구현체 Text와 Picture 클래스 모두 다 수정해야하는 단점이 있다. 더욱 문제가 있는 것은 실행하는 main메서드가 컴파일이 안된다.
```java
public static void main(String[] args) {
  List<Post> posts = Arrays.asList(new Text(), new Picture());
  List<SNS> sns = Arrays.asList(new Facebook(), new Twitter(), new Linkedin());
  posts.forEach(p -> sns.forEach(s -> p.postOn(s)));
}
위의 코드를 작성해보면 컴파일 에러가 발생한다. p.postOn(s) 이부분에 에러가 발생하는데 에러를 보자면 SNS 타입을 받는 메서드를 찾을 수 없다고 나온다. 왜 일까? 메서드 오버로딩은 정적 디스패치를 한다. 런타임 시점이 아니라 컴파일하는 시점에 파라미터의 타입을 정확히 체크를 해서 해당하는 메서드를 정해놔야 한다. 하지만 우리는 SNS라는 추상화된 객체를 넣어서 컴파일 타임에 에러가 발생한 것이다. 까다롭다. 이대로 포기 하면 안된다.
기존 코드도 변경하지 않고 좀 더 확장성있게 만들 수는 없을까? 그래서 나왔다. 더블 디스패치 라는 것이다.

interface Post {
  void postOn(SNS s);
}

static class Text implements Post {
  @Override
  public void postOn(SNS s) {
    s.post(this);
  }
}

static class Picture implements Post {
  @Override
  public void postOn(SNS s) {
    s.post(this);
  }
}

interface SNS {
  void post(Text text);
  void post(Picture picture);
}

static class Facebook implements SNS {
  @Override
  public void post(Text text) {
    System.out.println("text - facebook");
  }

  @Override
  public void post(Picture picture) {
    System.out.println("picture - facebook");
  }
}

static class Twitter implements SNS {
  @Override
  public void post(Text text) {
    System.out.println("text - twitter");
  }

  @Override
  public void post(Picture picture) {
    System.out.println("picture - twitter");
  }
}
```
기존의 코드들은 냅두고 두번째가 타입을 결정해야 되는 Facebook 클래스와 Twitter 클래스로 비지니스 로직을 옮겨놨다. 그리고 Post를 구현하고 있는 클래스에는 s.post(this) 이와 같이 자기 자신을 파라미터로 넘겨주면 된다.
한마디로 첫번째 호출 되는 Post쪽에서 타입을 결정하고 두번째로 넘기는 그런 방식이다. 그래서 디스패치를 두번한다고 더블 디스패치라고 한다.
만약 여기서 아까와 같이 SNS가 추가 되었다고 가정하자. 그럼 우리는 또다른 SNS 클래스를 구현만 해주면 된다.

```java
static class Linkedin implements SNS {
  @Override
  public void post(Text text) {
    System.out.println("text - linkedin");
  }

  @Override
  public void post(Picture picture) {
    System.out.println("picture - linkedin");
  }
}
```
위와 같이 Linkedin 클래스만 작성해서 구현해주면 된다. Post 인터페이스와 그에 따른 구현체들은 수정할 필요 없이 좀 더 확장성 있게 만들 수 있게 되었다.

출처: http://aoruqjfu.fun25.co.kr/index.php/post/1490 [머루의개발블로그]