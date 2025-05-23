# Initialization

## A phenomenon and a question

execute:

~~~java
public class HashMapInitialization {
    public static void main(String[] args) {
        // 创建 HashMap 对象 Sites
        HashMap<Integer, String> Sites0 = new HashMap<Integer, String>();
        HashMap<Integer, String> Sites1 = new HashMap<Integer, String>(5);
        HashMap<Integer, String> Sites2 = new HashMap<Integer, String>(5, 0.7F);


        System.out.println(Sites0.size());
        System.out.println(Sites1.size());
        System.out.println(Sites2.size());
    }
}
~~~



output:

~~~
0
0
0
~~~



==why?==

I "new" a HashMap with capacity 5, but the size of it is still zero.



source code:

~~~java
    /**
     * Constructs an empty <tt>HashMap</tt> with the specified initial
     * capacity and load factor.
     *
     * @param  initialCapacity the initial capacity
     * @param  loadFactor      the load factor
     * @throws IllegalArgumentException if the initial capacity is negative
     *         or the load factor is nonpositive
     */
    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }

    /**
     * Constructs an empty <tt>HashMap</tt> with the specified initial
     * capacity and the default load factor (0.75).
     *
     * @param  initialCapacity the initial capacity.
     * @throws IllegalArgumentException if the initial capacity is negative.
     */
    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }

    /**
     * Constructs an empty <tt>HashMap</tt> with the default initial capacity
     * (16) and the default load factor (0.75).
     */
    public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }
~~~





==But how to comprehend the source code?== 

What does such construct function really do?



## Diving into JVM



JVM:

> 2.9. [Special Methods](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.9:~:text=2.9.-,Special%20Methods,-At%20the%20level)
>
> At the level of the Java Virtual Machine, every ==constructor written== in the Java programming language (JLS §8.8) appears as an *instance ==initialization method==* that has the special name `<init>`. This name is supplied by a compiler.
>
> 5.5. [Initialization](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html#jvms-5.4.5:~:text=mA.-,5.5.%C2%A0Initialization,-Initialization%20of%20a)
>
> *Initialization* of a class or interface consists of executing its class or interface initialization method ([§2.9](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.9)).



As we can see, the constructor is seen as initialization method.



==But what does initialization method really do?==

What does initialize mean in the perspective of the computer?



## Class Loading Mechanism



JVM:

>  [Chapter 5. Loading, Linking, and Initializing](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html#jvms-5.4.5:~:text=Chapter%C2%A05.-,Loading%2C%20Linking%2C%20and%20Initializing,-Table%20of%20Contents)  
>
>  The Java Virtual Machine dynamically ==loads, links and initializes== classes and interfaces. 
>
>  1. Loading is the process of finding the binary representation of a class or interface type with a particular name and *creating* a class or interface from that binary representation. 
>  2. Linking is the process of taking a class or interface and combining it into the run-time state of the Java Virtual Machine so that it can be executed. 
>  3. Initialization of a class or interface consists of executing the class or interface initialization method `<clinit>` ([§2.9](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.9)).
>
>  In this chapter, [§5.1](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html#jvms-5.1) describes how the Java Virtual Machine derives symbolic references from the binary representation of a class or interface. [§5.2](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html#jvms-5.2) explains how the processes of loading, linking, and initialization are first initiated by the Java Virtual Machine. [§5.3](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html#jvms-5.3) specifies how binary representations of classes and interfaces are loaded by class loaders and how classes and interfaces are created. Linking is described in [§5.4](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html#jvms-5.4). [§5.5](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html#jvms-5.5) details how classes and interfaces are initialized. [§5.6](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html#jvms-5.6) introduces the notion of binding native methods. Finally, [§5.7](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html#jvms-5.7) describes when a Java Virtual Machine exits.



**As we can see, `Initializing` a class or interface consists of executing the class or interface initialization method. Before that, we need `Linking` to make class or interface executable. But where are those class or interface come from? `Loading`, which finds and derives symbolic references from binary representation of them with a particular name, creates the class or interface.**



So, that's take an insight into Class Loading Mechanism.

# Todo: an explanation of Class Loading Mechanism

