# What is MVC (Model View Controller) ?
## Definition:

In today's world, web applications has been changed from just simple html pages with a bit of CSS, to more complex applications with thousands of developers working on them. To make the work easier, developers use differnt models, one the model which is used quite extensively is MVC (`Model` `View` `Controller`). The goal of this pattern is to split a large application into different sections (MVC), that have their own purpose.

Lets see each section separately:

1. `Model` - This section controls the data part of the application. In other words it works with the database and provides the data as requested.
2. `View` - This section controls the presentation part of the application.
3. `Controller` - This acts as an interpretor between "model" and "view", and also the user.

## Example:

Suppose, a user requests the list of all modules in the Javascript section, to an application. See the image below:

![MVC pattern](https://media.geeksforgeeks.org/wp-content/uploads/20220224172049/Model2.png)

Step 1 : this request is sent to the `controller` section.
Step 2 : the `controller` section then sends this request to the `model` section, which in-return sends the list (as requested by the user), to the `controller` back.
Step 3 : the `controller` then sends this list sent by `model` section, to the the `view` section.
Step 4 : the `view` section transforms the data into a presentable format, and sends it back to the `cotroller`.
Step 5 : the `controller` then renders the data sent by the `view` section to the user. 

## Pseudocode:

http://yourapp.com/users/profile/1

/routes
    user/profile/:id = Users.getProfile(id)

/controllers
    class Users{
        function getProfile(id){
            profile = this.UserModel.getProfile(id)

            renderView('users/profile', profile)
        }
    }

/models
    class UserModel{
        function getProfile(id){
            data = this.db.get('SELECT * FROM users WHERE id = id')
            return data;
        }
    }

/views
   /users
      /profile
          <h1>{profile.name}</h1>
          <ul>
            <li>Email : {profile.email}</li>
            <li>Phone: {profile.phone}</li>     
          </ul> 


# Spring framework:

## Overview

The Spring Framework is an open-source application framework. It provides a comprehensive programming and configuration model for modern applications.

## Features

- **Dependency Injection (DI)**: Manages the dependencies between objects, promoting loose coupling and easier testing.
- **Aspect-Oriented Programming (AOP)**: Allows separation of cross-cutting concerns such as logging, security, and transaction management.
- **Spring MVC**: A robust framework for building web applications following the MVC pattern.
- **Spring Boot**: Simplifies the setup and development of new Spring applications by providing defaults and auto-configuration.
- **Data Access**: Simplifies data access using JDBC, ORM frameworks like Hibernate, and other data access technologies.
- **Transaction Management**: Provides a consistent programming model for transaction management.

## Spring MVC

Spring MVC is a web framework that provides infrastructure support for developing web applications. It follows the MVC design pattern and provides a clean separation between the model, view, and controller.

## Features of Spring MVC 

- **DispatcherServlet**: Acts as the front controller, managing the entire request handling process.
- **Controller**: Handles user requests and interacts with the model to process them.
- **Model**: Holds the business data and logic.
- **View**: Renders the model data, often using technologies like JSP, Thymeleaf, or FreeMarker.


# Conclusion 

The MVC architecture and the Spring Framework are essential tools for developing modern, robust, and scalable applications. MVC helps in separating concerns and organizing the application efficiently, while the Spring Framework provides a comprehensive set of features to simplify enterprise application development.


# Reference

- https://www.techtarget.com/whatis/definition/model-view-controller-MVC
- https://www.youtube.com/watch?v=DUg2SWWK18I
- https://www.youtube.com/watch?v=pCvZtjoRq1I
- https://www.youtube.com/watch?v=gq4S-ovWVlM