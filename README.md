# Service Class

## Learning Goals

- Create a service class.
- Add the service as a reference in the controller class.

## Introduction

Sometimes we want to add some more business logic to our applications, like
processing the client's parameters more than what we have been doing. Although we
could write all of this custom logic in a controller, it would make it difficult
to maintain and obfuscate the main purpose of the controller - which is to handle
the requests and responses.

We can use the `@Service` annotation, a specialization of the `@Component`
annotation, to indicate a **service class**. Service classes are used to implement
business logic, which can then be called by the controller. They serve as a
service layer and will keep the controller classes cleaner.

## Add the Service Class

When we created our spring-web-demo project, we created a class called
`LunchService.java`. Let's revisit that Java file by opening it up in IntelliJ and
adding the following code:

```java
package com.example.springwebdemo.service;

import org.springframework.stereotype.Service;

@Service
public class LunchService {
    public String getDailySpecial(String day) {
        String special;
        switch (day.toLowerCase()) {
            case "monday":
                special = "BLT Sandwich";
                break;
            case "tuesday":
                special =  "Veggie Burger";
                break;
            case "wednesday":
                special = "Pizza";
                break;
            case "thursday":
                special = "Chili";
                break;
            case "friday":
                special = "Mac and Cheese";
                break;
            default:
                special = "Not open for lunch";
        }
        return special;
    }
}
```

As we can see in the code above, we are writing the logic to meet the last
requirement: tell the customer the daily lunch special. We don't really want this
`switch` statement in the controller class, as it would clutter the class and
distract from the purpose of the controller. Therefore, we can place this type of
business logic in the service class.

## Using the Service Class

Now that we have our service class established, we can use it in our controller
class.

Consider the following modifications to the controller class:

```java
package com.example.springwebdemo.controller;

import com.example.springwebdemo.service.LunchService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class LunchController {

    // Add a reference to the LunchService
    private final LunchService lunchService;
    
    // Add a constructor
    @Autowired
    public LunchController(LunchService lunchService) {
        this.lunchService = lunchService;
    }

    @GetMapping("/")
    public String index() {
        return "Welcome to Spring Boot!";
    }

    @GetMapping("/greet")
    //localhost:8080/greet?name=Ted
    public String greet(@RequestParam(defaultValue = "customer") String name) {
        return String.format("Greetings %s!", name);
    }

    @GetMapping("/thank")
    //localhost:8080/thank?name=Ted
    public String thank(@RequestParam(defaultValue = "customer") String name) {
        return String.format("Thanks %s! Have a great day!", name);
    }
    
    @GetMapping("/lunch-special/{day}")
    //localhost:8080/lunch-special/monday
    public String getLunchSpecial(@PathVariable String day) {
        // Call the getDailySpecial method from the LunchService
        return lunchService.getDailySpecial(day);
    }
}
```

We'll first add a reference to the `LunchService` in the controller class and then
pass it into a constructor. Notice we have introduced a new annotation called
`@Autowired` before declaring the `LunchController` constructor. The `@Autowired`
annotation is a way to instantiate the `lunchService` variable using "Spring
Magic." The `LunchService` object will be automatically created and injected by
Spring into the `LunchController` class since we are using the `@Autowired`
annotation. We'll cover exactly how this works more in a few lessons.

Now we can actually use the `lunchService` in our controller class to call the
`getDailySpecial()` method that we added! We'll create a new method called
`getLunchSpecial` in our controller class that will take in a day of the week as
a parameter and return the value of `getDailySpecial()`. Instead of using the
`@RequestParam` annotation, like we used in the last lesson, we'll introduce a new
annotation called `@PathVariable`. The `@PathVariable` annotation maps the
dynamic path value to the parameter `day`. For example, if we want to know what
the daily lunch special is for Friday, the request URL would look like this:
"http://localhost:8080/lunch-special/friday" where we are dynamically assigning
the parameter `day` with the value `friday`.

### Run the Application

Let's run the application again!

This time, let's open a browser and type in
"http://localhost:8080/lunch-special/friday" into the URL. When the browser loads,
we should see this:

![lunch-special-friday](https://curriculum-content.s3.amazonaws.com/spring-mod-1/service/lunch-special-friday.png)

Mac and cheese is the meal we specified that should be returned when the day is
Friday! Let's try this out with another day of the week. Let's try Saturday. Type
in "http://localhost:8080/lunch-special/saturday" into the URL:

![lunch-special-saturday](https://curriculum-content.s3.amazonaws.com/spring-mod-1/service/lunch-special-saturday.png)

Looks like the cafeteria is closed for lunch on Saturday.

Go ahead and try this on your own and make sure that the lunch special seems
correct for the other days of the week!

## Conclusion

We have added a service class to the project which allows us to separate
business logic from controller logic and make our application more maintainable.

## References

- [Service Annotation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Service.html)
- [Autowired Annotation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)
- [GetMapping Annotation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/GetMapping.html)
- [PathVariable Annotation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/PathVariable.html)