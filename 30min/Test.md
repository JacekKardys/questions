# Part One - Java Sanity Check [5min]

## **String Comparison in Java:**
```java
// 1
System.out.println("Test" == "Test");                   
// 2
System.out.println("Test".equals("Test"));              
// 3
System.out.println("Test" == new String("Test"));       
// 4
System.out.println("Test".equals(new String("Test")));  
// 5
System.out.println("TestTest" == ("Test" + "Test"));    
// 6
String a = "Test";
System.out.println("TestTest" == (a + "Test"));
```

## How does Java achieve memory management? What are the key components of Java's memory model?

# Part Two - Coding Problems [15min]

## Write a function that reverses the order of words in a given sentence, but keeps the words themselves unchanged.

### Concepts: Easy. String manipulation, split(), String.join()

### Example:
Input:  `"Hello world this is Java"`
Output: `"Java is this world Hello"`

### Constraints:
- Words are separated by single spaces.
- No leading or trailing spaces.

# Find the Most Frequent Element

Given an array of integers, return the most frequently occurring element. If there are multiple, return any.

### Example:
Input:  `[1, 3, 3, 2, 1, 3, 2, 2, 2]`
Output: `2`

### Constraints:
- Solve in **O(n)** time.
- The array is non-empty.

# Part Three - Frameworks Supplementary Questions

Spring
    ** How does Spring Dependency Injection (DI) work? DI vs IoC (Inversion of Control)?
    ** How does Spring Boot auto-configuration work?
    ** What are the scopes of a Spring Bean?

Kafka
    ** How does Kafka guarantee message ordering?
        * What happens if a Kafka producer sends messages without a **partition key**?
    ** Explain the concept of idempotency
    ** How would you ensure exactly-once message processing in Kafka?
        * What happens if a consumer **dies** before committing an offset?
        * What is the difference between **earliest vs latest** offset strategies?

CI/CD
    ** What CI and CD?
    ** Explain a typical CI/CD pipeline for a Java application
    ** How do you manage environment variables in CI/CD pipelines?
    ** Does Docker improve CI/CD anyhow? If yes how if no why?