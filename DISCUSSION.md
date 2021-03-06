# Discussion

This file contains some comments about decisions and tradeoffs I made.

## Backend

### Limitations
* No currency conversion for past or present, i.e. $1 = £1 = 1€ = ...
* Dummy data can be inconsistent. E.g. a user who has made the pledge in 2017 may have donations 
and incomes from previous years.
* No tests for resource classes
* An income can only have one currency and one amount

### Choice of framework
This is my first project with Laravel. I gave it a try because it looked less verbose and 
more opinionated than Symfony. It has a lot of handy conventions and a few unique featuers 
like Valet. The things I missed most are:
* The clear separation of domain logic and application logic (as in Doctrine ORM)
* Automatic migration file generation based on class properties (as in Doctrine Migrations)
* Inside-class annotations for routing, authorization, persistence, etc.
* Nelmio ApiDoc Bundle 

I followed this tutorial here: https://www.toptal.com/laravel/restful-laravel-api-tutorial

### Design choices

The backend has three resource types: User, Donation and Income. I tried to design it according 
to the Domain Driven Design approach. The pledge isn't an independent entity because it is 
fully defined by its immutable percetage property (as with sums of money or colors, there's no 
inherent identity that stays constant over time even if you change properties). As there can't be 
Users without a pledge in the system, the pledge percentage is a non-nullable property of the 
User. The date of the initial pledge must coincide with the creation date of the User entity.

As users can make higher pledges in the course of their careers, the pledge percentage is also 
part of the Income resource (I assume that you can only go up with the pledge percentage at the 
end of a year).

## Frontend

### Limitations
* Creation, Update and Deletion of resources not implemented
* Currency in the paragraph below the pledge heading is always USD
* The JavaScript code could need some refactoring
* No tests

### Choice of framework
I used jQuery for making queries to the backend. If the frontend wasn't required to be a
single-page webapp, I'd have used a server-side template engine and perhaps pjax. I tend
to do without more powerful frontend libraries (React, Angular, Ember) when there's no
compelling reason for the domain logic to reside in the frontend, mainly because frontend 
libraries (except jQuery) tend to have shorter shelf-lives and often oblige you to 
duplicate some parts of your backend code (e.g. model, validation logic, etc.). 
For webapps that do need a fat client, I mainly used the Cappuccino Framework and 
the Ratatosk REST client in the past. For future projects that need to support mobile 
browsers and/or native mobile apps, I'd probably use Ionic.

