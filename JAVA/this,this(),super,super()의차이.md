## this와 this()

### this
`this`는 현재 클래스의 인스턴스, 지금 실행상태에 있는 인스턴스의 특정 필드를 지정할때 사용한다.

```java
class Test {
 int age;
    public void setAge(int age){
        this.age = age;
        }
 }
```

아래와 같은 코드를 작성하면 `setAge(int)` 메소드가 작동한다. `setAge(int age);` 메소드를 보면 파라미터로 전달된 int의 이름인 `age`와 `Test` 클래스의 필드값이 `age`로 같은 이름이다. 이럴때 `this`를 붙여주면 `this.age`는 **이 age는 지금 메소드(setAge)의 파라미터인 age값이 아니라 클래스의 필드인 age다** 라는 것을 나타낸다. 또한, `this`는 클래스 필드임을 확실히 해두기 위해 사용된다.

```java
Test test = new Test();
test.setAge(20);
```

### this()

`this()`는 생성자를 부르기위해 사용된다.

```java
class Person{

 int age;
 String name;
 String address;

 public Person(){
    this(0, null, null);
 }

 public Person(int age){
    this(age, null, null);
 }

 public Person(int age, String name){
    this(age, name, null);
 }

 public Person(int age, String name, String address){
    this.age = age;
    this.name = name;
    this.address= address;
     }
 }
```

한 사람에 대한 나이, 이름, 주소를 저장하는 클래스이다. 생성자를 보면 점차 파라미터가 늘어나는 것을 볼 수 있다. 다른 클래스에서 `Person jane = new Person(23,"jane","America");` , `Person jane = new Person(23,"jane");` 등 이와 같은 방법으로 인스턴스를 생성할 수 있도록 여러종류의 생성자를 마련했을때 `this();`가 유용하게 쓰일 수 있다.

생성자끼리도 서로가 서로에게 메시지를 전달할 수 있는 방법이 `this()`를 활용하는 것이다. **같은 방식으로 `super();`는 현재 자식 클래스가 자신을 생성할때 부모 클래스의 생성자를 불러서 함 초기화 해주고나서 자신을 초기화하는 것이다.**

## super와 super()

`super`는 키워드와 `super()` 메소드로 구분된다.

### super 키워드
super 키워드는 부모 클래스로부터 상속받은 필드나 메소드를 자식 클래스에서 참조하는 데 사용하는 참조 변수이다. 

인스턴스 변수의 이름과 지역 변수의 이름이 같은 경우 인스턴스 변수 앞에 `this` 키워드를 사용하여 구분할 수 있다. 이와 마찬가지로 부모 클래스의 멤버와 자식 클래스의 멤버 이름이 같을 경우 `super` 키워드를 사용하여 구별할 수 있다.

이렇게 자바에서는 `super` 참조 변수를 사용하여 부모 클래스의 멤버에 접근할 수 있다. `this`와 마찬가지로 `super` 참조 변수를 사용할 수 있는 대상도 인스턴스 메소드뿐이며, 클래스 메소드에서는 사용할 수 없다.

```java
class Parent { int a = 10; }

class Child extends Parent {
    void display() {
        System.out.println(a); // 10
        System.out.println(this.a); // 10
        System.out.println(super.a); // 10
    }
}

public class Inheritance02 {
    public static void main(String[] args) {
        Child ch = new Child();
        ch.display();
    }
}
```

`int`형 변수 `a`가 부모 클래스인 Parent 클래스에서만 선언되어 있기 때문에, 지역변수와 `this`참조 변수 그리고 `super` 참조 변수 모두 같은 값을 출력한다.

```java
class Parent {
    int a = 10;
}

class Child extends Parent {
    int a = 20;

    void display() {
        System.out.println(a); // 20
        System.out.println(this.a); // 20
        System.out.println(super.a); // 10
    }
}

public class Inheritance03 {
    public static void main(String[] args) {
        Child ch = new Child();
        ch.display();
    }
}
```

`int`형 변수 `a`가 부모 클래스인 Parents 클래스와 자식 클래스인 Child 클래스에도 선언되어 있다. 지역변수와 `this` 참조 변수는 자식 클래스에 대입된 값을 출력하고, `super` 참조 변수만 부모 클래스에서 대입된 값을 출력하게 된다.

**쉽게 설명하면, 자식클래스가 내 클래스안에서 찾지말고 부모클래스에서 찾으라고 알려주는게 `super`라고 알고 있으면 된다.**

### suepr() 메소드

`this()` 메소드가 같은 클래스의 다른 생성자를 호출할 때 사용된다면, `super()` 메소드는 부모 클래스의 생성자를 호출할 때 사용된다.

부모 클래스의 멤버를 초기화하기 위해서는 자식 클래스의 생성자에서 부모 클래스의 생성자까지 호출해야만 한다. 그 이유는 자식 클래스의 인스턴스를 생성하면, 해당 인스턴스에는 자식 클래스의 고유 멤버뿐만 아니라 부모 클래스의 모든 멤버까지 포함되어 있기 때문이다. 이러한 부모 클래스의 생성자 호출은 모든 클래스의 부모 클래스인 Object 클래스의 생성자까지 계속 거슬러 올라가며 수행된다.

**위와 같은 이유로 자바 컴파일러는 부모 클래스의 생성자를 명시적으로 호출하지 않는 모든 자식 클래스의 생성자 첫 줄에 다음과 같은 명령문을 추가하여, 부모 클래스의 멤버를 추기화할 수 있도록 해준다.**

> super();

하지만 자바 컴파일러는 컴파일시 클래스에 생성자가 하나도 정의되어 있지 않아야만, 자동으로 기본 생성자를 추가해 준다. 만약 다음 예제처럼 부모 클래스에 매개변수를 가지는 생성자를 하나라도 선언했다면, 부모 클래스에는 기본 생성자가 자동으로 추가되지 않을 것이다.

```java
class Parent {
    int a;
    Parent(int n) { a = n; }
}
```

만약 다음 예제 처럼 Parent 클래스를 상속받은 자식 클래스에서 `super()` 메소드를 사용하여 부모 클래스의 기본 생성자를 호출하게 되면, 오류가 발생하게 될 것이다.

```java
class Parent {
    int a;
    Parent(int n) { a = n; }
}

class Child extends Parent {

    int b;

    Child() {
        super();
        b = 20;
    }
```

왜냐하면 부모 클래스인 **Parent 클래스에는 기본 생성자가 추가되어 있지 않기 때문이다.** 따라서 매개변수를 가지는 생성자를 선언해야 할 경우에는 되도록이면 다음 예제처럼 기본 생성자까지 명시적으로 선언하는 것이 좋다. *쉽게말하면!!! `super();`를 사용하면 부모 클래스의 생성자 `Parent(){;}`를 불러준다는 것! 만약 부모클래스에 매개변수를 가지는 생성자를 하나라도 선언되어서 자동으로 기본 생성자가 추가되지 않았을 경우에는 명시적으로 기본생성자를 선언해주는 것이 좋다.*

```java
class Parent {
    int a;

    Parent() { a = 10; }
    Parent(int n) { a = n; }
}

class Child extends Parent {
    int b;

    Child() {
        super();
        b = 20;
    }
```

#### super() 메소드의 호출

```java
class Parent {
    int a;

    Parent() { a = 10; }
    Parent(int n) { a = n; }
}

class Child extends Parent {
    int b;

    Child() {  
        super(40); // 두 번째 생성자에 의해 a = 40이 됨
        b = 20;
    }

    void display() {
        System.out.println(a); // 40
        System.out.println(b); // 20
    }
}

public class Inheritance04 {

    public static void main(String[] args) {
        Child ch = new Child();
        ch.display();
    }
}
```