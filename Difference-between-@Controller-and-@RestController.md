# Java Spring Framework @Controller vs @RestController

Inthis article, we will see the difference between @Controller and @RestController Annotations in Spring boot/Spring MVC.

## Overview
The @Controller annotations is used for traditional spring MVC framework Controller for long time.The @RestController annotation was introduced in Spring 4.0 to simplify the creation of RESTful web services.
The @RestController annotation in Spring MVC/Spring BOOT is nothing but a combination of @Controller and @ResponseBody annotation.
It was added into Spring 4.0 to make the development of RESTful Web Services in Spring framework easier.
If you know REST web services you know that the fundamental difference between a REST API and a web application.
That is the response from a web application is generally view (HTML + CSS) because they are intended for human viewers.
REST API just returns data in form of JSON or XML because most of the REST clients are programs. This difference is also obvious in the @Controller and @RestController annotation.
## Difference between @Controller and @RestController in Spring MVC/BOOT
1. The @Controller is a annotation to mark class as Controller Class in Spring While @RestController is used in REST Web services and similar to @Controller and @ResponseBody.
2. The @Controller annotation indicates that the class is controller like web Controller while @RestController annotation indicates that the class is controller where @RequestMapping Method assume @ResponseBody by Default(i.e REST APIs).
3. The key difference is that you do not need to use @ResponseBody on each and every handler method once you annotate the class with @RestController.
4. @Controller create a Map of Model Object and find a view while @RestController simply return object and object data directly written into http response as JSON orXML.
This can be also done with @Controller annotation and @ResponseBody annotation but since this is the default behaviour of RESTful Web Services. Spring introduced @RestController which is combined behaviour of @Controller and @ResponseBody together.
### Example
Following are Example Code which are Equal in Spring.
@ Controller
```
@Controller
@ResponseBody
Public class MVCController{
}
```
@ RestController
```
@RestController
public class MVCController{
}
```
Above Two code are equal in Spring MVC/Spring Boot.
### Conclusion
Today we have understand the difference between @Controller and @RestController annotation of Spring MVC/Spring Boot.