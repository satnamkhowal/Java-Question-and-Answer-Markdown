# Why do we use Interface in the service layer instead of normal class?

## When to Use Interfaces in Your Application

There are several advantages in utilizing the features of Interfaces in general programming. As you may already know, interfaces define a set of functionality as a rule or a contract. When you implement an interface all of these functionality must be implemented in the concrete class.

The ideas discussed here can be applied using any programming language that supports Interfaces. Java has been used in examples for simplicity.

In general when one writes a simple program, one may not think about the need of using an Interface. But when you are building a larger system or a library which keeps evolving, it is a good idea to use Interface. A particular advantage of using interface in Java is that it allows multiple inheritance.

The full power of Interface is utilized when dependency injection techniques is used to inject required implementation on run time. Using references to interfaces instead of their concrete implementation classes helps in minimizing ripple effects, as the user of an interface reference doesn't have to worry about the changes in the underlying concrete implementation.

To give an example consider our application has a layered architecture with Controller layer on top then Service layer, Repository layer and domain layer in that order. Here controller layer interacts with service layer and service layer interacts with repository layer. Each layer is aware of the services provided by the layer below it. We expose this information using Interface and hide the concrete implementation.

Let us say we have a UserRegistrationService, which registers a new user in our system. Our requirement is to save the user information in a relational database and also in xml files. If we were not using interface, the UserRegistrationService may be implemented with two functions saveToXML() and saveToDatabase().
``` java
public class UserRegistrationService {
    public void saveToXML(UserInfo userInfo) {
        //save to xml using service exposed by Repository layer
    }
    public void saveToDatabase(UserInfo userInfo) {
        //save to db using service exposed by Repository layer
    }
}
```
In this case, the UserRegistrationController should be aware of the concrete implementation of these two functions in UserRegistrationService to use them.
``` java
//Controller becomes complicated when additional features are required
public class UserRegistrationController {
    //Controller should be aware of the implementation when no Interface is used
    UserRegistrationService userRegistrationService = new UserRegistrationService();
    public void processRequest(UserInfo userInfo) {
        this.saveToXml(userInfo);
    }
    private void saveToXml(UserInfo userInfo) {
        userRegistrationService.saveToXml(userInfo);
    }
    private void saveToDatabase(UserInfo userInfo) {
        userRegistrationService.saveToDatabase();
    }
}
```
If an additional functionality to save the information as JSON is required then you will have to add a new function saveToJson() in the Service class as well as make changes in the Controller. This adds lots of complication to maintenance of our huge application with hundreds of controllers and services.

When using interface this becomes much simpler. We define our UserRegistrationService like this:

``` java
public interface UserRegistrationService {
    public void save();
}
```
The controller layer is only aware of this interface, which has a save method.

Let us say we have two implementations like the following:
``` java
public class UserRegistraionServiceXmlImpl implements UserRegistrationService {
    @Override
    public void save(UserInfo userInfo) {
        //save to xml using service exposed by Repository layer
    }
}
public class UserRegistraionServiceRelDbImpl implements UserRegistrationService {
    @Override
    public void save(UserInfo userInfo) {
        //save to relational db using service exposed by Repository layer
    }
}
```
Here we can choose any of these two implementation on run time using dependency injection. One may use @Inject or @Resource annotation to mark an implementation is to be injected. If using Spring, one may also use Xml bean definitions.
``` java
//Controller becomes much simpler when using Interfaces in the service layer
public class UserRegistrationController {
    @Resource(name = "userRegistrationServiceXmlImpl")
    UserRegistrationService userRegistrationService;
    public void processRequest(UserInfo userInfo) {
        userRegistrationService.save(userInfo);
    }
    public void setUserRegistrationService(UserRegistrationService userRegistrationService) {
        this.userRegistrationService = userRegistrationService;
    }
}
```
Now when we need to add the additional functionality of saving to Json, we just add another implementation and select that implementation to be injected. (You may use an IOC{Inversion Of Control} container like Spring to achieve this seamlessly.)
``` java
public class UserRegistraionServiceJsonImpl implements UserRegistrationService {
    @Override
    public void save() {
        //save to json using service exposed by Repository layer
    }
}
```
This highly reduces the software modification and extension cost. As changes in one layer does not effect other layer and new functionalities are made available to other layer immediately.

Thus using interface gives you more power over extending and maintaining your application, utilize abstraction and implement good software development practices.



## Why to use Service Layer in Spring MVC

There has been ambiguity about the use of Service layer in Spring ,also it is very difficult to find information on internet about the usage of service-layer.
In this blog we will learn the usage of Service layer.As usual we will walk through a small example to understand the concept.
Anybody who has started learning Spring MVC will be aware how model,controller,View interact with each other as part of a Spring MVC Application.
For someone who has fundamental knowledge of MVC framework must be knowing that Controller interacts with DAO layer to persist data to the Database.
Generally the DAO layer should be as light as possible and should exist solely to provide a connection to the DB, sometimes abstracted so different DB backends can be used together.
The service layer is there to provide logic to operate on the data sent to and from the DAO and the client. Very often these 2 pieces will be bundled together into the same module, and occasionally into the same code, but you’ll still see them as distinct logical entities.
### Reasons to use :
1. #### Provides separation of concern-
Service layer provides code modularity,the business logic and rules are specified in the service layer which in turn calls DAO layer ,the DAO layer is then only responsible for interacting with DB.
2. #### Provides Security -
If you provide a service layer that has no relation to the DB, then it is more difficult to gain access to the DB from the client except through the service. If the DB cannot be accessed directly from the client (and there is no trivial DAO module acting as the service) then an attacker who has taken over the client cannot have access to your data directly.
3. #### Provide Loose Coupling-
Service layer can also be used to serve loose coupling in the application.Suppose your controller has 50 methods and in turn it calls 20 Dao methods,Now at later point you decide to change the Dao methods serving these controllers.You need to change all the 50 methods in controller. Instead if you have 20 service methods calling those 20 Dao methods, you need to make change in only 20 Service methods to point to a new Dao.

![Why%20to%20use%20Service%20Layer%20in%20Spring%20MVC-1](assets/img/Why%20to%20use%20Service%20Layer%20in%20Spring%20MVC-1.png)


Flow of an application using Service layer
Let’s take an implementation case how Service layer can be used in an application.
Consider a customer management system where you can perform basic adding ,updating ,deleting,listing of customer .


![Why%20to%20use%20Service%20Layer%20in%20Spring%20MVC-2](assets/img/Why%20to%20use%20Service%20Layer%20in%20Spring%20MVC-2.png)

Flow of Demo App without Service Layer
So if you see in the diagram above ,Browser sends the request to Controller,then it passes the control to DAO layer to access data from Database,the data received is then used for rendering View on the browser.
This pattern involves lot of risk as we are exposing our DB connection to the controller class,also if we want to do some business processing then we have to write all the code in Controller class which is not a good practice. So instead it is preferred to use Service layer in between the Controller and DAO layer.Service layer will have some business logic for our customers and in turn it will call DAO class to interact with the Database.

![Why%20to%20use%20Service%20Layer%20in%20Spring%20MVC-3](assets/img/Why%20to%20use%20Service%20Layer%20in%20Spring%20MVC-3.png)

``` java

@Controller
@RequestMapping("/customer")
public class CustomerController {

	@Autowired
	private  CustomerService  customerService;
	
	//@RequestMapping("/list")
	@GetMapping("/list")
	public String listcustomer(Model themodel) {
		themodel.addAttribute("customers",customerService.getCustomer());
		
		return "list-customer";
	}
	
	
	@GetMapping("/addcustomer")
	public String addCustomer(Model themodel) {
		System.out.println("in addCustomer ");
		Customer customer=new Customer();
		themodel.addAttribute(customer);
		return "show-customer-form";
	}
	
	@PostMapping("/processform")
	public String processForm(Model themodel,@ModelAttribute("customer") Customer newcustomer ) {
		
		
		//adding logic here
		customerService.addCustomer(newcustomer);
		
		//display
		themodel.addAttribute("customers",customerService.getCustomer());
		
		System.out.println("in process form");
		return "list-customer";
	}
	
	
	@GetMapping("/update")
	public String updateCustomer(Model themodel,@RequestParam("customerId") int theId ) {
		
		themodel.addAttribute("customer",customerService.getSingleCustomer(theId));
		
		return "show-customer-form";
		
		
	}
	
	@GetMapping("/delete")
	public String deleteCustomer(Model themodel,@RequestParam("customerId") int theId ) {
		

				//adding logic here
				customerService.deleteCustomer(theId);
		
		
				//display
				themodel.addAttribute("customers",customerService.getCustomer());
				
				System.out.println("in process form");
				return "list-customer";
		
		
	}
	
}

```
Flow of Demo App With Service Layer.
In context of our Demo App ,We have a Customer controller class which will get the request from the browser,according to the request appropriate method of controller will be called and processing will be done.

Customer Controller
Now we will call the Service “CustomerServiceImpl” ,it has Customerdao object which is autowired automatically as the class is loaded.Here @Service annotation is used over CustomerService class to mark the class as a service provider. We have annotated it with @Service annotation so that spring context can autodetect it and we can get its instance from the context.


``` java
@Service
public class CustomerServiceimpl implements CustomerService {

	@Autowired
	private CustomerDao customerdao;
	
	@Transactional
	public List<Customer> getCustomer() {
		
                // do some business processing here ...
    		//now call DAO layer
		return customerdao.getCustomer();

  }
	
	
	@Transactional
	public boolean addCustomer(Customer customer) {
		
   		 // do some business processing  here ...
    		//now call DAO layer
		return(customerdao.addCustomer(customer));
		
	}
	
	@Transactional
	public Customer getSingleCustomer(int theId) {
		
		// do some business processing here ... 
    		//now call DAO layer
		return customerdao.getSingleCustomer(theId);
	}
	
	
	@Transactional
	public void deleteCustomer(int theId) {
   		 // do some business processing here ...
   		 //now call DAO layer
		customerdao.deleteCustomer(theId);
	}
}
```


```java


@Repository
public class CustomerDaoimpl implements CustomerDao{

	@Autowired
	public SessionFactory sessionfactory;
	
	@Override
	public List<Customer> getCustomer() {
	Session currentsession=sessionfactory.getCurrentSession();
	
	List<Customer> customers=currentsession.createQuery("from Customer",Customer.class).getResultList();
	
	return customers;
	}
	
	@Override
	public Boolean addCustomer(Customer customer) {
	
		try {
			Session currentsession=sessionfactory.getCurrentSession();
			
			currentsession.save(customer);
				
		} catch (Exception e) {
			// TODO: handle exception
			return false;
		}
		
		return true;
	}

	
	@Override
	public Customer getSingleCustomer(int theId) {
		Session currentsession=sessionfactory.getCurrentSession();
		
		return currentsession.get(Customer.class,theId);
	}
	
	
	@Override
	public void deleteCustomer(int theId) {
		Session currentsession=sessionfactory.getCurrentSession();
		
		currentsession.delete(getSingleCustomer(theId));
	}
}
```

Service class

Customer Dao class
As the flow reaches here a transactional method will be invoked and it will perform its task.Suppose getCustomer() is called then it will provide all the customers from the Database. Notice how CustomerDAO object is injected into Service class and CustomerServiceImpl object is injected into Controller class and controller uses the Service object to access Dao layer .Finally CustomerDaoImpl uses the instance of sessionfactory bean to create a session and persist data into Database.