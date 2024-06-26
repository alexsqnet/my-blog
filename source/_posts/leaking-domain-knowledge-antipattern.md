---
title: Leaking domain knowledge to tests - Anti-pattern
date: 2024-06-17 00:56:35
tags: 
    - unit testing
    - anti-pattern
category:
    - unit testing

thumbnail: uploads/leaking-domain-knowledge-antipattern/cover.png
banner: uploads/leaking-domain-knowledge-antipattern/cover.png
---
![Alt Text](uploads/leaking-domain-knowledge-antipattern/cover.png)
Leaking domain knowledge to tests is an anti-pattern in software development and testing. It occurs when the test code contains information about the implementation details of the system under test (SUT) rather than focusing on its behavior and expected outcomes. This can lead to a number of issues that undermine the effectiveness and maintainability of the tests.

<!--more-->

It usually, takes place in tests that cover complex algorithms.
Let's look at the following simple example.

Example 1:
``` csharp
public static class Calculator
{
    public static int Add(int value1, int value2)
    {
        return value1 + value2;
    }
}
```
We have the ***Calculator*** class with a single ***Add*** method. 
Let's try to write an unit test.

Example 2: An incorrect way ❌
``` csharp
public class CalculatorTests
{
    [Fact]
    public void Adding_two_numbers()
    {
        int value1 = 1;
        int value2 = 3;
        int expected = value1 + value2;   //   <--- The leakage
        
        int actual = Calculator.Add(value1, value2);
        Assert.Equal(expected, actual);
    }
}
```
It looks fine, but, ***it is an anti-pattern*** because we repeat the algorithm from the ***Calculator*** class in our test method:
``` csharp
int expected = value1 + value2;
```
Imagine that some changes will be made to the algorithm of the ***Add*** method from the ***Calculator*** class. Therefore, it will be necessary to make the same change in the unit test. So this is the biggest problem: Weak resistance to refactoring.

How to test the algorithm properly, then !?
So, instead of duplicating the algorithm, hard-code its expected results into the test, as demonstrated in the following example.

Example 3: A correct way ✔️

``` csharp
public class CalculatorTests
{
    [Theory]
    [InlineData(1, 3, 4)]
    [InlineData(11, 33, 44)]
    [InlineData(100, 500, 600)]
    public void Adding_two_numbers(int value1, int value2, int expected)
    {
        int actual = Calculator.Add(value1, value2);
        Assert.Equal(expected, actual);
    }
}
```

This example I found in Vladimir Khorikov's book - 'Unit Testing Principles, Practices, and Patterns'. It is indeed highly recommended for anyone interested in improving their understanding and practice of unit testing. It provides valuable insights, practical advice, and best practices for writing effective unit tests.

Key Characteristics of the Antipattern

1. ***Implementation Details in Tests***: Tests include specifics about how the code is implemented rather than what it should do. For example, knowing the exact data structures or internal workings of the functions being tested.

2. ***Tight Coupling***: Tests become tightly coupled to the implementation. If the implementation changes, even if the external behavior remains the same, the tests break and need to be updated.

3. ***Reduced Readability***: Tests become harder to understand because they include complex implementation details that are not necessary for understanding the expected behavior.

4. ***Fragility***: Tests become fragile and more prone to failure due to minor changes in the codebase that do not affect the overall functionality but alter the internal implementation.

5. ***Limited Refactoring***: Developers are discouraged from refactoring code, as such changes would require corresponding changes in the test code, making refactoring a more daunting task.