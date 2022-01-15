---
layout: post
comments: true
description:
categories: [clean-code, testing]
last_modified_at: 2022-01-02T20:52:08.052481
last-modified-purpose:
permalink: /clean-fast-unit-tests/
title: Writing Clean and Fast Unit Tests
---

Clean tests are as important as clean code. Same kind of principles apply here.

1. Test only what is neccessary
2. Keep code clean

while TDD, is shown to keep test cases to a minimum, this post explains some other ways tests can be organize

# Group Tests Together

You should group your tests together. This would make it easier to find your tests

Junit and Jest both allow for grouping of tests.
- In JUnit 5 you can use @NestClasses
- In Jest you can use describe 

## Example:

### JUnit:

```java
class MainTestClass

    @Nested
    @DisplayName("Group of tests")
    class MethodNameTests(){

        @Test
        @DisplayName("Test Name")
        class tests{
            ...
        }
    }
```

### Jest:

```js

describe("Group of tests", ()=>{

    it("test name", ()=>{


    })
})

```

# Have global mocks/stub object

Your tests should be as small as possible. Rather than create objects in every test method, Create a dummy stub and use it in all subsiqueny tests

In Junit you can have @BeforeEach and @BeforeAll annotations to have methods run before each test case or test class.

# Mock object

## Naming

In case I am testing a positive response, I usually name variable as such.

If it is a variable which is bad, prefix it with bad

example:
- idealHttpHeader
- goodFuture

If it's a bad response,

Example:
- badHttpHeader
- badHttpHeaderWithoutContentType
- badFuture

## Never generate random inputs. Always use fixed values. [^5]

# Start with `method_should_<>_when/with_<>()` or `itShould_<>_When_<>`

This makes you focus on exactly what you are testing.

Though you can remove the should part too.

Example:
- GroupName: Add function
- TestName: Should work with negative values: add_shouldWork_withNegativeValues
- TestName: Should work with positive values: add_shouldWork_withPositiveValues
- TestName: Should work with floating point values: add_shouldWork_withFloatingPoint
- TestName: Should raise exception when input is null: add_shouldRaiseException_whenInputisNull

# Given When Then

Given When then is how most tests are written.

Start the tests which fixing the input values and mocking -> Given
Call the function in test -> when
Assert Responses -> then

# Size of tests should be less than size of your display

This is a useful guideline which applies to a normal clean code too.

## Assertions

Best Practice: One logical assertion per test method. [^4]

Have clear error messages [^5]

Aim for asserting behavior and not implementation detail. More coupled your tests are with implementation, harder the refactoring would be.[^6]

# Writing tests that run fast

## Time complexity of tests shoud be O(1) [^1]

Cyclomatic time complexity of your tests should be O(1).
- No loops
- No if statements

## For Spring Boot

### Initialize only beans you need using `@ContextConfiguration` [^3]

Using `@ContextConfiguration` won't start the whole spring infrastructure. This making your tests faster.

[^1]: [YouTube: Continous Delivery: The 3 Types of Unit Test in TDD](https://www.youtube.com/watch?v=W40mpZP9xQQ)
[^2]: [Stack Overflow: Unit Test Best Practices](https://stackoverflow.com/a/7398606)
[^3]: [YouTube: Defog Tech: Spring Boot - Testing basics](https://www.youtube.com/watch?v=Ekr4jxOIf4c)
[^4]: [Unit Testing Best Practices: 9 to Ensure You Do It Right](https://www.testim.io/blog/unit-testing-best-practices/)
[^5]: [YouTube: DevOxx: Effective Unit Testing by Eliotte Rusty Harold](https://www.youtube.com/watch?v=fr1E9aVnBxw) (This includes tips on debugging and refactoring)
[^6]: [Cucumber: Blog: Getting Started with BDD (Part 1)](https://cucumber.io/blog/bdd/getting-started-with-bdd-part-1/)