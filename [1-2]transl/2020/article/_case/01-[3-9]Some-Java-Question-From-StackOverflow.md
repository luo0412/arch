# [3-9]Some-Java-Question-From-StackOverflow

- Grammar
- @doc 
  - https://stackoverflow.com/questions/tagged/java
  - https://stackoverflow.com/questions/tagged/spring
  - https://stackoverflow.com/search?q=spring+boot

---

# [3-1]Java Grammar

- Is Java “pass-by-reference” or “pass-by-value”?
  - https://stackoverflow.com/questions/40480/is-java-pass-by-reference-or-pass-by-value

```
Java is always pass-by-value
```

- Avoiding NullPointerException in Java
  - https://stackoverflow.com/questions/271526/avoiding-nullpointerexception-in-java

```
assert <condition>

@Nullable and @NotNull
```

- How do I generate random integers within a specific range in Java?
  - https://stackoverflow.com/questions/363681/how-do-i-generate-random-integers-within-a-specific-range-in-java

```
ThreadLocalRandom.current().nextInt(min, max + 1);
```

- When to use LinkedList over ArrayList in Java? @diff
  - https://stackoverflow.com/questions/322715/when-to-use-linkedlist-over-arraylist-in-java

```
LinkedList implements it with a doubly-linked list
ArrayList implements it with a dynamically re-sizing array
```

- How do I convert a String to an int in Java?
  - https://stackoverflow.com/questions/5585779/how-do-i-convert-a-string-to-an-int-in-java

```
import com.google.common.primitives.Ints;

int foo = Optional.ofNullable(myString)
 .map(Ints::tryParse)
 .orElse(0)
```

- Does a finally block always get executed in Java?
  - https://stackoverflow.com/questions/65035/does-a-finally-block-always-get-executed-in-java

```java
// The only times finally won't be called ar
1) System.exit
2) Runtime.getRuntime().halt(exitStatus)
3) JVM crashes first
4) JVM reaches an infinite loop (or some other non-interruptable, non-terminating statement) in the try or catch block
5) kill -9 <pid>
6) power failure, hardware error, OS panic
7) daemon??

===
public static void main(String[] args) {
    System.out.println(Test.test());
}

public static int test() {
    try {
        return 0;
    }
    finally {
        System.out.println("finally trumps return.");
    }
}
```

- What is a `JavaBean` exactly?
  - https://stackoverflow.com/questions/3295496/what-is-a-javabean-exactly

```
A JavaBean is just a standard
```

- How to round a number to n decimal places in Java
  - https://stackoverflow.com/questions/153724/how-to-round-a-number-to-n-decimal-places-in-java

```java
new BigDecimal(String.valueOf(double)).setScale(yourScale, BigDecimal.ROUND_HALF_UP)
    
===    
DecimalFormat df = new DecimalFormat("#.####");
df.setRoundingMode(RoundingMode.CEILING);
for (Number n : Arrays.asList(12, 123.12345, 0.23, 0.1, 2341234.212431324)) {
    Double d = n.doubleValue();
    System.out.println(df.format(d));
}
```

# [3-3]Spring

- What exactly is Spring Framework for? [closed] @digest
  - https://stackoverflow.com/questions/1061717/what-exactly-is-spring-framework-for

```
Basically Spring is a framework for dependency-injection 
which is a pattern that allows building very decoupled systems
```

- Why is my Spring @Autowired field null? @digest @todo
  - https://stackoverflow.com/questions/19896870/why-is-my-spring-autowired-field-null

```
The field annotated @Autowired is null 
because Spring doesn't know about the copy of MileageFeeCalculator 
that you created with new and didn't know to autowire it
```

