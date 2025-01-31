# WARMUP ##############################################

"Test" == "Test" 	  				//1
"Test".equals("Test") 				//2
"Test" == new String("Test") 		//3
"Test".equals(new String("Test")) 	//4

# (Groovy) ##############################################

var city = "";
var user = entityLoadByPK("User",10);

if (user != null){
if(user.getAddress() != null) {
if(user.getAddress().getCity() != null) {
city = user.getAddress().getCity();
}
}
}

return city;

return user?.address?.city

# Java core ##############################################

What is the difference between Arraylist and Linkedlist? or How does the Hashmap work? What you need to make it work properly?

possibility of null in hashMap and hashSet
What is the behaviour of this statement - Optional.ofNullable(null); -  Wrong answer: He said it will throw a Null pointer exception.
What is the key difference between Optional.orElseGet() and Optional.orElse()? - No idea
Are singleton beans thread-safe? - Wrong answer: He said yes.
Do you know design patterns? What is strategy?
What is the difference between BeanFactory and ApplicationContext? -  No idea
What is Dependency Injection?
What is a JSON web token?
What is a Pod in Kubernetes?
What are the challenges you face while working Microservice Architectures?
JUnits, integration tests
mockito - how to return one of the arguments in the when statement + void

interface SomeService {

	void process(String data); 		//1
	String process(String data);	//2
}

@Mock
private SomeService service;

//throw exception for 1 method in stub
//return always first argument for 2 method in stub


###############################################
What is a default method and when do we use it?
Immutability in Java
StringBuilder vs String Buffer
synchronized block
Callable vs Runnable
possibility of null in hashMap and hashSet
what lambda expression is
Functional Interface
Try with resources
Streams
annotation: Configuration
bean scopes
default bean scope
annotation : requestBody , requestParam
JUnits, integration tests
mockito - how to return one of the arguments in the when statement + void
spy()
rebase vs merge
Difference between hash map vs hash table. - Wrong answer
What is the behaviour of this statement - Optional.ofNullable(null); -  Wrong answer: He said it will throw a Null pointer exception.
What is the key difference between Optional.orElseGet() and Optional.orElse()? - No idea
What is the difference between BeanFactory and ApplicationContext? -  No idea
Are singleton beans thread-safe? - Wrong answer: He said yes.
What is the difference between access token and authorization code? - Wrong answer.
What is a JSON web token?

What is a security context?
What is OWASP? (List some of the security problems)
What is SQL injection?
What is dependencyManagement component used for?
What are Microservices ?
What are the benefits of Kubernetes?
What is a Pod in Kubernetes?
What are the challenges you face while working Microservice Architectures?