# **Part Three - Frameworks & Supplementary Skills**

## **Spring Framework Questions**

### **️⃣ How does Spring Dependency Injection (DI) work? DI vs IoC (Inversion of Control)?**

✅ **Expected Answer:**
- **Inversion of Control (IoC):** A design principle where the control of object creation and dependency resolution is **shifted from the application code to a framework (Spring Container).**
- **Dependency Injection (DI):** A way to implement IoC by injecting dependencies **rather than creating them manually.**
- Spring **automatically manages dependencies** using:
    - **Constructor Injection** (recommended, immutable dependencies)
    - **Setter Injection** (mutable dependencies)
    - **Field Injection** (not recommended due to testability issues)
- **Spring Context** (via `ApplicationContext`) manages the lifecycle of beans and their dependencies.
- Uses **annotations** (`@Component`, `@Service`, `@Repository`, `@Autowired`) or explicit **XML/Java configuration**.

---

### **2️⃣ What are the scopes of a Spring Bean?**
✅ **Expected Answer:**
- **Singleton** (default) – One instance per application context.
- **Prototype** – New instance every time requested.
- **Request, Session, Application** – Web scopes for managing state per HTTP request/session.

👉 **Follow-up:**
- How does **Spring handle Singleton beans in a multi-threaded environment?**
- Can we force a `@Bean` to be lazily initialized?

---

### **3️⃣ How does Spring Boot auto-configuration work?**
✅ **Expected Answer:**
- Uses **Spring Boot Starters** (pre-configured dependencies).
- `@SpringBootApplication` enables **component scanning, configuration, and auto-configuration**.
- Reads `META-INF/spring.factories` to auto-configure beans based on **classpath dependencies**.

👉 **Follow-up:**
- How can we **disable** a specific auto-configuration? (`@EnableAutoConfiguration(exclude = X.class)`)
- Why is `application.properties`/`application.yml` used in Spring Boot?

---

## **Kafka Questions**

### **4️⃣ How does Kafka guarantee message ordering?**
✅ **Expected Answer:**
- **Partition Key** determines message ordering within a **single partition**.
- Kafka guarantees order **only within a partition**, not across multiple partitions.

👉 **Follow-up:**
- What happens if a Kafka producer sends messages without a **partition key**?
- How does **Kafka handle retries** if a consumer fails?

---

### **5️⃣ Explain Kafka Consumer Group & Offset Management.**
✅ **Expected Answer:**
- **Consumer Group:** A set of consumers working together to consume messages from **different partitions**.
- **Offset Management:** Consumers store offsets in **Kafka (`__consumer_offsets`)** or externally (Zookeeper).
- **Commits:** Automatic (`enable.auto.commit=true`) vs Manual (`commitSync()` or `commitAsync()`).

👉 **Follow-up:**
- What happens if a consumer **dies** before committing an offset?
- What is the difference between **earliest vs latest** offset strategies?

---

### **6️⃣ How would you ensure exactly-once message processing in Kafka?**
✅ **Expected Answer:**
- **Idempotent Producers (`acks=all`)** prevent duplicate writes.
- **Transactional API (`enable.idempotence=true`)** ensures atomic writes across topics.
- **Consumer-side deduplication** using unique message IDs.

👉 **Follow-up:**
- How do Kafka transactions differ from **at-least-once vs at-most-once** guarantees?
- What role does **Kafka Streams** play in exactly-once processing?

---

## **CI/CD & DevOps Questions**

### **7️⃣ Explain a typical CI/CD pipeline for a Java application.**
✅ **Expected Answer:**
- **CI (Continuous Integration):** Developers push code → Triggers automated tests via Jenkins/GitHub Actions/GitLab CI.
- **CD (Continuous Deployment/Delivery):**
    - Artifacts (`.jar`, Docker image) built, stored in **Nexus/Artifactory**.
    - Deployment to **Staging → Prod** (via Kubernetes, Ansible, or Terraform).

👉 **Follow-up:**
- What’s the difference between **Continuous Deployment vs Continuous Delivery**?
- Why should we **cache dependencies** in a CI pipeline?

---

### **8️⃣ How do you manage environment variables in CI/CD pipelines?**
✅ **Expected Answer:**
- **Secrets Management:** `.env` files, Kubernetes Secrets, AWS Secrets Manager, HashiCorp Vault.
- **Parameter Store:** Pass environment variables dynamically via GitHub Actions, Jenkins, or Kubernetes ConfigMaps.

👉 **Follow-up:**
- How do you **secure API keys** in a CI/CD pipeline?
- Why is it a bad practice to store secrets in a `Dockerfile`?

---

### **9️⃣ How does Docker improve CI/CD pipelines?**
✅ **Expected Answer:**
- **Consistency:** Works the same in dev/test/prod.
- **Isolation:** Avoids dependency conflicts using containers.
- **Scalability:** Works well with **Kubernetes & microservices**.

👉 **Follow-up:**
- What’s the difference between **Docker Compose vs Kubernetes**?
- How do you optimize **Docker image size** in production?
