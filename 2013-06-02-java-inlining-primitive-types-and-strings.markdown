Title: Java: inlining primitive types and Strings
Date: 2013-06-02 17:47
Tags: java

At this moment, almost all IDEs and build systems can and actively use an increment build, i.e. recompile only changed files; so one time you could face the following issue: you change value of a constant in a one class, but get original value when use it in the another class.

Assume we have the piece of code in 2 class files correspondingly:

```java
public class A {
	public final static String s = "A";
	public final static int i = 1;
}

public class B {
	public static void main(String[] args) {
		System.out.println("A.s = " + A.s + ", A.i = " + A.i);
	}
}
```

After compiling classes and running, we get output:

```bash
$ javac A.java && javac B.java && java B
A.s = A, A.i = 1
```

In case of changing constants in A, recompiling it (only A.java) and then running B, we will receive the previous result. It's because compiler goes through constants in the class and checks each member whether or not it is an expression possible to compute at the compile time or the run time. A constant with the compile time computable value is a target for inlining.

Let's edit the A class int the following way:

```java
public class A {
        public final static String s = "A".intern();
        public final static int i = 1 + (int) Math.min(0, System.currentTimeMillis());
}
```

And then recompile and run:

```bash
$ javac A.java && javac B.java && java B
A.s = A, A.i = 1
```

In case of changing constants in A and recompling only A, every running B we will receive correct constants values.
You can easily find what are the differencies in the bytecodes of the two versions of produced classes by using standart JDK utility javap.

For example, for the first version of A class we get:

```bash
$ javap -c A
Compiled from "A.java"
public class A {
  public static final java.lang.String s;

  public static final int i;

  public A();
    Code:
       0: aload_0       
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return        
}
```

Welcome to the amazing modern Java World.