# Service Class

## Learning Goals

- Define a service class
- Create a service class.

## Introduction

We may need to modify and shape data to complete user queries. We also need to
process parameters that the client may provide. Although we could write all of
our custom logic in a controller, it would make it difficult to maintain and
obfuscate the controller’s main methods.

A service class is used for implementing business logic which can then be called
by the controller. The service class has access to repositories and models, so
it can utilize them for custom logic.

## Create a MemberService Class

We will be using the `@Service` annotation to indicate a service class. It’s a
specialization of the `@Component` class.

Create a `Service` class in the `service` package and add the following code:

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
