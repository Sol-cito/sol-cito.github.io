---
layout: post
title: "[OOP] SOLID (1) - Single Responsibility"
subtitle: "A class should exist for only one responsibility"
background: "/img/bg_technology.jpg"
categories: technology-oop
---

### SOLID Principle - Single Responsibility

The most important premise of software engineering that all developers should bear in mind is that in reality, software applications are to keep changing. 

Software is built under the social trend and requisites from customers, which means the architecture of the application must be as flexible as possible to those continuously changing requirements. Namely, **how to properly cope with these changes and smoothly maintain the software** has always been the most significant topic for developers.

The SOLID principle has come out as a great strategy to tackle this issue in Object Oriented methodology, so I would like to point out each principle with example codes for myself to keep that in mind.

<br/>

#### Responsibility of Class

The world of Java consists of numerous classes. Which field variables and functions each class is totally up to the architect and developer. However, it doesn't mean that a class has anything it wants to have without any rules. 

The rule we must consider for architecture is that **a class should have only one job to do**.

Let's consider an order class in food delivery software.

```java
class Order {
    private String customerName;
    private ArrayList<Food> foodList;
    private Address address;
    
    public void addFood(Food food){
        this.foodList.add(food);
    }
    
    public void cancelOrder(){
        // blah blah..
    }
    
    // and there are many functions..
}
```

The class has its own field and functions related to the action of order such as food list, address and customer info, etc. 

One day, a new requirement is piled on the desk from the marketing team and they ask to add a function of logging all the food that has been selected.

So a new method loggingFoodList is added like below...

```java
class Order {
    private String customerName;
    private ArrayList<Food> foodList;
    private Address address;
    
    public void addFood(Food food){
        this.foodList.add(food);
    }
    
    public void cancelOrder(){
        // blah blah..
    }
    
    // new method
    public void loggingFoodList(Logfile logfile){
        logfile.write(this.foodList);
    }
}
```

This action completely violates the single responsibility principle because **logging is not the order class's responsibility.**

It can cause various problems such as

- The same method should be added to the other classes like delivery, payment, and discount. It is quite a burden and also produces duplicate code blocks across classes.
- As a result, when the requirement for logging changes (logging contents, format, level, etc.) those methods should be all re-coded.
- If the marketing team requires another type of printing out method, developers should create a new method in each class and the size of a class will be too big to properly maintain.

To prevent this trouble that causes overtime work, **we have to separate logging functions and create a new class that is particularly responsible for logging.** 

```java
// Create new class
Class Logger{
    private Logfile logfile;
    
    public void loggingItems(ArrayList<Item> list){
        this.logfile.wirte(list);
    }
}

public class Application {
    public static void main(String[] args) {
		Order order = new Order();
        order.addFood(Apple apple);
        
        // separate logging function
        Logger logger = new Logger();
        logger.loggingItems(order.foodList);
        
        // another class
        FreeEventFood fef = new FeeEventFood();
        Food food = fef.selectRandomFood();
        logger.loggingItems(food);
        
    }
}

```

By creating a Logger class and clarifying each class's responsibility, it became possible to separately maintain the logging function and now it is way easier to respond to changing requirements of logging.

So how can we define 'responsibility'?

Sadly, the scope of responsibility depends on the scale of software and architecture. What we know for sure though, is that .**the actor** of each class can give you a hint of responsibility.

For example, the order class above is to be used by customers whereas the logger class is for the marketers. 

That being said, each responsibility should be assigned to each actor, and that is why the relationship between actors and classes should always be deeply considered during the software architecture.

