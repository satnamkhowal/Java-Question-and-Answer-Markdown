## Choosing from multiple beans in the context



 


In this article, we discuss the scenario in which Spring needs to inject a value into a parameter or class field but has multiple beans of the same type to choose from.

Take 40% off Spring Quickly by entering fccspilca2 into the discount code box at checkout at manning.com.

In this article, we discuss the scenario in which Spring needs to inject a value into a parameter or class field but has multiple beans of the same type to choose from. Say you have three Parrot beans in the Spring context. You configure Spring to inject a value of type Parrot into a parameter. How does Spring behave? Which of the beans of the same type does the framework choose to inject in such a scenario?

Depending on your implementation, you get the following cases:

The identifier of the parameter matches the name of one of the beans from the context (which is the same as the name of the method annotated with @Bean that returns its value). In this case, Spring chooses that bean for which the name is the same as the parameter.
The identifier of the parameter doesn’t match any of the bean names from the context and you have the following options:
You marked one of the beans as primary. In this case, Spring selects the primary bean for injection.
You can explicitly select a specific bean using the @Qualifier annotation.
If none of the beans are primary and you don’t use @Qualifier either, the app fails with an exception, complaining of the fact that the context contains more beans of the same type, and Spring doesn’t know which one to choose.
Let’s try further, in project gsws-app3-ex1, a scenario in which we have more than one instance of a type in the Spring context. Listing 1 shows you a configuration class that defines two Parrot instances and uses injection through the method parameters.

## Listing 1. Using parameter injection when the context contains more than one bean of a kind

 ``` java
 @Configuration
 public class ProjectConfig {
  
   @Bean
   public Parrot parrot1() {
     Parrot p = new Parrot();
     p.setName("Koko");
     return p;
   }
  
   @Bean
   public Parrot parrot2() {
     Parrot p = new Parrot();
     p.setName("Miki");
     return p;
   }
  
   @Bean
   public Person person(Parrot parrot2) {    #A
     Person p = new Person();
     p.setName("Ella");
     p.setParrot(parrot2);
     return p;
   }
 }
 ```
  
### #A The name of the parameter matches the name of the bean representing parrot Miki.

Running the app with this configuration, you’d observe a console output similar to the next code snippet. Observe that Spring linked the person bean to the parrot named Miki because the bean representing this parrot has the name parrot2 (figure 1).

 ```
 Parrot created
 Person's name: Ella
 Person's parrot: Parrot : Miki
  
```

![choosing-beans-based-on-context-in-spring_01](assets/img/choosing-beans-based-on-context-in-spring_01.png)

### Figure 1. One way to instruct Spring to provide you a specific instance from its context, when the context contains more than one instance of the same type, is to rely on the name of this instance. Name the parameter the same as the instance you’d like Spring to provide you.

In a real-world scenario, I prefer to avoid relying on the name of the parameter. The name of the parameter could be easily refactored and changed by mistake by another developer. To feel more comfortable, I usually choose a more visible approach to express my intention to inject a specific bean: using the @Qualifier annotation. Again, in my experience I have heard developers arguing for and against using the @Qualifier annotation. For example, I feel more comfortable and I consider it better to use in this case because it clearly defines your intention. Other developers consider adding this annotation creates unneeded (boilerplate) code.

In listing 2, you find an example of using the @Qualifier annotation. Observe that instead of having a specific identifier of the parameter, now I specify the name of the bean I want to inject using the value attribute of the @Qualifier annotation.

## Listing 2. Using the @Qualifier annotation

 ``` java
 @Configuration
 public class ProjectConfig {
  
   @Bean
   public Parrot parrot1() {
     Parrot p = new Parrot();
     p.setName("Koko");
     return p;
   }
  
   @Bean
   public Parrot parrot2() {
     Parrot p = new Parrot();
     p.setName("Miki");
     return p;
   }
  
   @Bean
   public Person person(
     @Qualifier("parrot2") Parrot parrot) {    #A
  
     Person p = new Person();
     p.setName("Ella");
     p.setParrot(parrot);
     return p;
   }
 }
  ```

### #A Using the Qualifier annotation, you clearly mark your intention to inject a specific bean from the context.

Rerunning the application, you’ll observe the app prints the same result into the console.

 ```
 Parrot created
 Person's name: Ella
 Person's parrot: Parrot : Miki
  ```

A similar situation can also happen when using the @Autowired annotation. To show you this case I created another project, which I named gsws-app3-ex2. In this project, we define two beans of type Parrot (using the @Bean annotation) and an instance of Person (using stereotype annotations). I’ll configure Spring to inject one of the two parrot beans in the bean of type Person.

As presented in the next code snippet, I didn’t add the @Component annotation to the Parrot class because I intend to define the two beans of type Parrot using the @Bean annotation in the configuration class.

 ```
 public class Parrot {
  
   private String name;
  
   // Omitted getters, setters, and toString() 
 }
  ```
We define a bean of type Person using the @Component stereotype annotation. Observe the identifier I gave to the parameter of the constructor in the next code snippet. The reason I gave the identifier “parrot2” is that this is the name I’ll also configure for the bean in the context which I want Spring to inject into that parameter.

 ```
 @Component
 public class Person {
  
   private String name = "Ella";
  
   private final Parrot parrot;
  
   public Person(Parrot parrot2) {
     this.parrot = parrot2;
   }
  
   // Omitted getters and setters
  
 }
  ```
  
I define two beans of type Parrot using the @Bean annotation in the configuration class. Don’t forget we still have to add @ComponentScan as well to tell Spring where to find the classes annotated with stereotype annotations. In our case, we annotated class Person with the @Component stereotype annotation. In listing 3, you find the definition of the configuration class.

## Listing 3. Defining the beans of type Parrot in the configuration class

 ``` java
 @Configuration
 @ComponentScan(basePackages = "beans")
 public class ProjectConfig {
  
   @Bean
   public Parrot parrot1() {
     Parrot p = new Parrot();
     p.setName("Koko");
     return p;
   }
  
   @Bean   
   public Parrot parrot2() {    #A
     Parrot p = new Parrot();
     p.setName("Miki");
     return p;
   }
 }
  ```
### #A With the current setup, this bean named parrot2 is the one that Spring injects into the Person bean.

What happens if you run a main method as the one presented in the next code snippet? Our person owns which parrot? Because the name of the constructor’s parameter matches the name of one of the bean in the Spring context (parrot2), Spring injects that bean (figure 2). The name of the parrot the app prints in the console’s Miki.

 ``` java
 public class Main {
  
   public static void main(String[] args) {
     var context = new
         AnnotationConfigApplicationContext(ProjectConfig.class);
  
     Person p = context.getBean(Person.class);
  
     System.out.println("Person's name: " + p.getName());
     System.out.println("Person's parrot: " + p.getParrot());
   }
 }
  
  ```
Running this app, the console shows the following output:

 ```
 Person's name: Ella
 Person's parrot: Parrot : Miki
  ```
  

![choosing-beans-based-on-context-in-spring_01](assets/img/choosing-beans-based-on-context-in-spring_01.png)


### Figure 2. When the Spring context contains multiple beans of the same type, Spring selects the bean whose name matches the name of the parameter.

As we discussed for the parameter of the @Bean annotated method, I recommend avoiding relying on the name of the variable. Instead, I prefer using the @Qualifier annotation in this case, to express my intention clearly on the fact that I inject a specific bean from the context. This way, we minimize the chance that someone might refactor the name of the variable and affect how the app works. Take a look at the change I’ve made to the Person class in the next code snippet. Using the @Qualifier annotation, I specify the name of the bean I want Spring to inject from the context, and I don’t rely anymore on the identifier of the constructor’s parameter. You find this change in the project named gsws-app3-ex3.
``` java
 
 @Component
 public class Person {
  
   private String name = "Ella";
  
   private final Parrot parrot;
  
   public Person(@Qualifier("parrot2") Parrot parrot) {
     this.parrot = parrot;
   }
  
  // Omitted getters and setters
  
 }
 ```
  
The behavior of the app doesn’t change, and the output remains the same, but using this approach, your code is less subjective to mistakes.

That’s all for this article.