# Service Class

## Learning Goals

- Create a service class.
- Autowire the service class.

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

```java
package org.example.springwebdemo.service;

import org.example.springwebdemo.model.Member;
import org.example.springwebdemo.repository.MemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class MemberService {
    @Autowired
    MemberRepository memberRepository;

    public Member createMember(Member member) {
        return memberRepository.save(member);
    }

    public List<Member> getMembers() {
        return memberRepository.findAll();
    }

    public Member getMember(Integer id) {
        return memberRepository.findById(id).get();
    }

    public Member updateMember(Integer id, Member memberData) {
        Member member = memberRepository.findById(id).get();
        member.setName(memberData.getName());
        member.setEmail(memberData.getEmail());
        return memberRepository.save(member);
    }

    public void deleteMember(Integer id) {
        memberRepository.deleteById(id);
    }
}
```

### MemberRepository

The `MemberRepository` object is automatically created and injected by Spring
into the `MemberService` class because of the `@Autowired` annotation.

### createMember Method

This method uses the default `save` method on the repository object to persist
the member in the database.

### getMembers Method

It uses the `findById` method on the repository to find a record with the ID of
`id` and returns the object.

### updateMember Method

We can’t simply use a default method for this method and have to add some custom
method instead. The controller will call this method with the ID of the member
(`id`) the client wants to update and the data (`memberData`) to update it with.
We update the `member` instance and call the `save` method to persist the
updated record.

### deleteMember

It removes the member with the ID of `id` from the database.

## Conclusion

We have added a service class to the project which allows us to separate
business logic from controller logic and make our application more maintainable.
