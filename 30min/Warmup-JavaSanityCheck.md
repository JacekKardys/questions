# Warm-up Questions for Java Technical Interview

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

### **Answer:**
```java
// 1 → true 
// Both "Test" are string literals, which are **interned** in the **String Pool**. 
// Since they refer to the **same object**, `==` returns `true`.

// 2 → true 
// `.equals()` compares **content**, and both strings are identical. 
// `"Test".equals("Test")` compares values, so it returns `true`.

// 3 → false 
// `new String("Test")` **creates a new object** in the **heap**, not in the **String Pool**. 
// `==` checks for **reference equality**, and these are **two different objects**, so it returns `false`.

// 4 → true 
// `.equals()` compares **content**, not references. 
// The values are identical, so it returns `true`.

// 5 → true 
// `"Test" + "Test"` is a **compile-time constant** because both operands are **string literals**. 
// The Java compiler **optimizes this to `"TestTest"` at compile time**. 
// Since `"TestTest"` is **interned** in the **String Pool**, both references point to the **same object**. 

// 6 → false 
// `a` is a **variable**, not a **string literal**. 
// Even though `a` contains `"Test"`, Java **cannot optimize `"Test" + a"` at compile time**. 
// Instead, Java **creates a new `String` object** in the **heap** at runtime. 
// The result is a **different object** than the interned `"TestTest"`, so `==` returns `false`.
```

### **Follow-up Questions:**
- What happens if we use .intern() on new String("Test") before comparison? (System.out.println("Test" == new String("Test").intern());)
- How does string interning improve memory efficiency?

## What happens if you override `equals()` but not `hashCode()`?

### **Answer:**
If `equals()` is overridden without overriding `hashCode()`, objects that are considered equal by `equals()` may have different hash codes. This violates the contract of `hashCode()`, leading to unexpected behavior in hash-based collections like `HashMap` and `HashSet`.

### **Follow-up Questions:**
- How does `HashMap` use `hashCode()` and `equals()` internally?
- What would happen if two different objects have the same `hashCode()`?

---
## How do Java records differ from regular classes?

### **Answer:**
- **Records** (`record Person(String name, int age)`) are immutable data carriers.
- **Automatically generate** `equals()`, `hashCode()`, and `toString()`.
- **No setters, only final fields**.

### **Follow-up Questions:**
- Can records have methods?
- How do records differ from Lombok’s `@Data`?

---
---
## How does Java achieve memory management? What are the key components of Java's memory model?

### **Answer:**
Java relies on automatic **garbage collection (GC)** to manage memory. The key components:
- **Heap**: Stores objects. Divided into **Young Generation** (Eden, Survivor spaces), **Old Generation**, and sometimes **Metaspace**.
- **Stack**: Stores method call frames, local variables, and references.
- **Metaspace (since Java 8)**: Stores class metadata instead of PermGen.

### **Follow-up Questions:**
- What are the differences between **G1 GC** and **Parallel GC**?
- How does the GC know when to remove an object? (Strong vs Weak references) (Default type of reference in Java, as long a strong reference exists... local vars, class fields etc)
- When would a memory leak occur in Java?

---
