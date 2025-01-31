# Part One - Java Sanity Check [5min]
## **String Comparison in Java:**
```java
System.out.println("Test" == "Test");                               // 1
System.out.println("Test".equals("Test"));                          // 2
        System.out.println("Test" == new String("Test"));                   // 3
        System.out.println("Test".equals(new String("Test")));              // 4
        System.out.println("TestTest" == ("Test" + "Test"));                // 5
String a = "Test"; System.out.println("TestTest" == (a + "Test"));  // 6
```
* *1*  → true
  * Both "Test" are string literals, which are **interned** in the **String Pool**. they refer to the **same object**
* *2* → true
  * `.equals()` compares **content**, and both strings are identical. `"Test".equals("Test")` compares values.
* *3* → false
  * `new String("Test")` **creates a new object** in the **heap**, not in the **String Pool**.
  * `==` checks for **reference equality**, and these are **two different objects**, so it returns `false`.
* *4* → true
  * `.equals()` compares **content**, not references. The values are identical.
* *5* → true
  * `"Test" + "Test"` is a **compile-time constant** because both operands are **string literals**.
  * The Java compiler **optimizes this to `"TestTest"` at compile time**.
  * Since `"TestTest"` is **interned** in the **String Pool**, both references point to the **same object**.
* *6* → false
  * `a` is a **variable**, not a **string literal**. Java **cannot optimize `"Test" + a"` at compile time**.
  * Instead, Java **creates a new `String` object** in the **heap** at runtime.
  * The result is a **different object** than the interned `"TestTest"`, so `==` returns `false`.

### **Follow-up Questions:**
- String literal vs object
  * String literal: Automatically interned at class load. Points to the same thing in the pool.
  * String object (via new): Always a new thing on the heap. Not interned unless .intern() is called.
- Is String Object ever automatically interned?
  * Nah, requires explicit .intern()
- What happens if we use .intern() on new String("Test") before comparison? (System.out.println("Test" == new String("Test").intern());)
- How does string interning improve memory efficiency?
  * Merges duplicates into one shared thing (pool). All references point to the same interned instance.

## ** Equals vs Hash Code

```java
@AllArgsConstructor
abstract class AbstractPerson {
  public final String name;
  public int id;
}

@EqualsAndHashCode(callSuper = true)
@AllArgsConstructor
class Person extends AbstractPerson { }

@AllArgsConstructor
class PersonWithoutEquals extends AbstractPerson {
  @Override
  public int hashCode() { return id; }
}

@EqualsAndHashCode(callSuper = true)
@AllArgsConstructor
class PersonWithoutHashCode extends AbstractPerson {
  @Override
  public int hashCode() {
    return System.identityHashCode(this); // Uses default 
  }
}

@EqualsAndHashCode(callSuper = true)
@AllArgsConstructor
class PersonMutable extends AbstractPerson {
  @Override
  public int hashCode() { return id; }
}

public class PersonTest {
  public static void main(String[] args) {
      
    Person p1 = new Person("Alice", 1);
    Person p2 = new Person("Alice", 1);
    Person p3 = new Person("Bob", 2);
    Person p4 = new Person("Charlie", 3);
    Person p5 = new Person("AL1ce", 1);

    System.out.println("=== EQUALS AND HASHCODE CONTRACT TESTS ===");

    System.out.println(p1.equals(p2));
    System.out.println(p1.hashCode() == p2.hashCode());

    System.out.println(p1.equals(p3));
    System.out.println(p1.hashCode() == p3.hashCode());

    System.out.println(p1.hashCode() == p5.hashCode());
    System.out.println(p1.equals(p5));

    PersonWithoutEquals pe1 = new PersonWithoutEquals("Alice", 1);
    PersonWithoutEquals pe2 = new PersonWithoutEquals("Alice", 1);
    
    System.out.println(pe1.equals(pe2));
    System.out.println(pe1.hashCode() == pe2.hashCode());

    // HashSet #1
    PersonWithoutHashCode ph1 = new PersonWithoutHashCode("Alice", 1);
    PersonWithoutHashCode ph2 = new PersonWithoutHashCode("Alice", 1);

    Set<PersonWithoutHashCode> hashSet = new HashSet<>();
    hashSet.add(ph1);
    hashSet.add(ph2);
    
    System.out.println(hashSet.size());

    // HashSet #2
    PersonMutable pm = new PersonMutable("Eve", 4);
    Set<PersonMutable> mutableSet = new HashSet<>();
    
    mutableSet.add(pm);
    
    System.out.println(mutableSet.contains(pm));

    pm.id = 5;
    
    System.out.println(mutableSet.contains(pm));
    System.out.println(mutableSet.size());

    // HashMap
    Map<Person, Integer> map = new HashMap<>();
    map.put(p1, 100);
    map.put(p2, 200);
    map.put(p3, 300);

    System.out.println(map.get(p1));
    System.out.println(map.get(p2));
    System.out.println(map.get(p3));
  }
}
```


## How does Java achieve memory management? What are the key components of Java's memory model?
Java relies on automatic **garbage collection (GC)** to manage memory. The key components:
- **Heap**: Stores objects. Divided into **Young Generation** (Eden, Survivor spaces), **Old Generation**, and sometimes **Metaspace**.
- **Stack**: Stores method call frames, local variables, and references.
- **Metaspace (since Java 8)**: Stores class metadata instead of PermGen.

### **Follow-up Questions:**
- What are the differences between **G1 GC** and **Parallel GC**?
- How does the GC know when to remove an object? (Strong vs Weak references) (Default type of reference in Java, as long a strong reference exists... local
  vars, class fields etc)
- When would a memory leak occur in Java?

# Part Two - Coding Problems [15min]

## Write a function that reverses the order of words in a given sentence, but keeps the words themselves unchanged.
### Concepts: Easy. String manipulation, split(), String.join()
### Example:
Input:  `"Hello world this is Java"`
Output: `"Java is this world Hello"`
### Constraints:
- Words are separated by single spaces.
- No leading or trailing spaces.

#### Streams one-liner
```java
public static String reverseWords(String sentence) {
  return Arrays.stream(sentence.split(" "))
          .collect(Collectors.collectingAndThen(Collectors.toList(), list -> {
            Collections.reverse(list);
            return String.join(" ", list);
          }));
}
```

#### Manual Swap no Collection reverse, No Collections API
```java
public static String reverseWords(String sentence) {
  String[] words = sentence.split(" ");
  int left = 0, right = words.length - 1;
  while (left < right) {
    String temp = words[left];
    words[left++] = words[right];
    words[right--] = temp;
  }
  return String.join(" ", words);
}
```

# Find the Most Frequent Element
Given an array of integers, return the most frequently occurring element. If there are multiple, return any.

### Example:
Input:  `[1, 3, 3, 2, 1, 3, 2, 2, 2]`
Output: `2`
### Constraints:
- Solve in **O(n)** time.
- The array is non-empty.

### Example using HashMap
```java
public static int mostFrequent(int[] nums) {
  Map<Integer, Integer> freq = new HashMap<>();
  int maxCount = 0, result = nums[0];

  for (int num : nums) {
    int count = freq.getOrDefault(num, 0) + 1;
    freq.put(num, count);
    if (count > maxCount) {
      maxCount = count;
      result = num;
    }
  }
  return result;
}
```

# **Part Three - Frameworks & Supplementary Questions**

## **Spring Framework Questions**
### **1️⃣ How does Spring Dependency Injection (DI) work? DI vs IoC (Inversion of Control)?**
✅ **Expected Answer:**
- **Inversion of Control (IoC):** A design principle where object creation & lifecycle management are delegated to Spring **instead of being manually handled in
  application code**.
- **Dependency Injection (DI):** One way to implement IoC where dependencies are **injected rather than created manually**.
- Spring supports:
  - **Constructor Injection** (preferred, immutable dependencies).
  - **Setter Injection** (mutable dependencies).
  - **Field Injection** (discouraged due to poor testability).
- Spring manages beans through **ApplicationContext** and annotations (`@Component`, `@Service`, `@Repository`).

👉 **Follow-up Questions:**
- What are the differences between `@Component`, `@Service`, and `@Repository`?
- Why is **field injection** considered bad practice?
- How does **Spring resolve circular dependencies in DI?**
- What happens if two beans of the **same type** exist in the Spring Context?

### **2️⃣ How does Spring Boot auto-configuration work?**
✅ **Expected Answer:**
- Auto-configuration **removes the need for manual bean definitions** by detecting dependencies on the classpath.
- `@SpringBootApplication` enables:
  - **Component scanning** (`@ComponentScan`).
  - **Configuration** (`@Configuration`).
  - **Auto-configuration** (`@EnableAutoConfiguration`).
- Uses **Spring Boot Starters** to pre-configure commonly used beans.
- `META-INF/spring.factories` specifies auto-configurations.

👉 **Follow-up Questions:**
- How can we **disable** specific auto-configuration?
- Why should we prefer **YAML (`application.yml`) over properties (`application.properties`)?**
- What are the risks of **Spring Boot Auto-configuration?**

### **3️⃣ What are the scopes of a Spring Bean?**
✅ **Expected Answer:**
| Scope | Description |
|-------------|------------|
| **Singleton** (default) | One instance per Spring Container. |
| **Prototype** | New instance every time requested. |
| **Request** | New instance per HTTP request. |
| **Session** | New instance per user session. |
| **Application** | Shared instance for a web app’s lifecycle. |

👉 **Follow-up Questions:**

- How does **Spring handle Singleton beans in multi-threaded applications?**
- When should we **use Prototype scope instead of Singleton?**
- Can we make a **Singleton bean thread-safe?** How?

---

## **Kafka Questions**

### **4️⃣ How does Kafka guarantee message ordering?**

✅ **Expected Answer:**

- **Kafka guarantees ordering within a single partition.**
- Messages with the **same partition key** are always written **sequentially** to the same partition.
- Ordering **is not guaranteed across multiple partitions**.

👉 **Follow-up Questions:**

- What happens if a **Kafka producer sends messages without a partition key?**  
  **(They are randomly assigned to partitions, losing strict ordering.)**
- How can we scale Kafka **while maintaining order**?

---

### **5️⃣ Explain the concept of idempotency.**

✅ **Expected Answer:**

- **Idempotency** ensures that duplicate messages do **not cause unintended side effects**.
- **Kafka Producer Idempotence (`enable.idempotence=true`)** ensures that a message **is not written twice** even if retries happen.
- **At consumer level:** Deduplicate messages using a **unique key or transaction ID**.

👉 **Follow-up Questions:**

- How does Kafka **handle retries** without breaking idempotency?
- Why is idempotency **important in event-driven architectures?**

---

### **6️⃣ How would you ensure exactly-once message processing in Kafka?**

✅ **Expected Answer:**

- **Producer Level:**
  - **Enable idempotence** (`enable.idempotence=true`).
  - Use **Kafka Transactions** to write atomically across multiple topics.
- **Consumer Level:**
  - **Read-Process-Commit Pattern:** Consume, process, and only commit if successful.
  - **Use External Idempotency Keys** (e.g., Store processed event IDs in Redis or DB).
- **Offset Management:**
  - Use **manual commits** (`commitSync()` or `commitAsync()`) instead of auto-commit.

👉 **Follow-up Questions:**

- What happens if a **consumer dies before committing an offset?**
- What is the difference between **earliest vs latest** offset strategies?
- How do Kafka transactions **differ from at-least-once vs at-most-once guarantees?**

---

## **CI/CD Questions**

### **7️⃣ What are CI and CD?**

✅ **Expected Answer:**
| Concept | Description |
|---------|------------|
| **CI (Continuous Integration)** | Automates testing & validation when code is merged. |
| **CD (Continuous Delivery)** | Automates deployment to **staging** but requires manual approval for production. |
| **CD (Continuous Deployment)** | Automates deployment to **production** with no manual intervention. |

👉 **Follow-up Questions:**

- Why is **CI/CD important in microservices architectures?**
- How does **CI/CD reduce deployment risks?**

---

### **8️⃣ Explain a typical CI/CD pipeline for a Java application.**

✅ **Expected Answer:**

1. **Developer pushes code to Git.**
2. **CI server (Jenkins/GitHub Actions/GitLab CI) triggers build.**
3. **Automated tests (unit, integration, security).**
4. **Build artifact (JAR/Docker Image) is stored in Artifactory/Nexus.**
5. **Deployment to Staging via Helm/Kubernetes.**
6. **Production Deployment (if Continuous Deployment is enabled).**

👉 **Follow-up Questions:**

- How do you **rollback a failed deployment** in a CI/CD pipeline?
- How can you speed up a **slow CI/CD pipeline?**
- Why use **immutable artifacts** in a CI/CD pipeline?

---

### **9️⃣ How do you manage environment variables in CI/CD pipelines?**

✅ **Expected Answer:**

- **Secrets Management:** Use Kubernetes Secrets, AWS Secrets Manager, HashiCorp Vault.
- **Environment Injection:** Use Jenkins, GitHub Actions, or Kubernetes ConfigMaps.
- **Avoid Hardcoding:** Never store credentials in Git repositories.

👉 **Follow-up Questions:**

- How do you **secure API keys** in a CI/CD pipeline?
- Why is storing secrets in a **Dockerfile** a bad practice?

---

### **🔟 Does Docker improve CI/CD? If yes, how? If no, why?**

✅ **Expected Answer:**
✅ **Yes, Docker improves CI/CD because:**

- **Consistency**: Ensures the same environment across dev, test, and prod.
- **Isolation**: Prevents dependency conflicts using containers.
- **Portability**: Works across different OS and infrastructures.

👉 **Follow-up Questions:**

- What is the difference between **Docker Compose vs Kubernetes**?
- How do you optimize **Docker image size** for production?
- Why should we use **multi-stage builds** in Docker