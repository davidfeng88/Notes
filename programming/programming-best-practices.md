# Programming - Best Practices

{% embed url="https://henrikwarne.com/2020/03/22/secure-by-design/" %}

#### DOMAIN PRIMITIVES

For example, say that you want to represent the number of books ordered. Instead of using an integer for this, define a class called _Quantity_. It contains an integer, but also ensures that the value is always between 1 and 240 \(if that’s the upper limit\). Or for a user name, instead of just using a string, define a class called _UserName_. It contains a string holding the user name, but also enforces all the domain rules for a valid user name. This can include minimum and maximum lengths, allowed characters etc.

Nothing in the domain should be represented by primitive types in the language \(int, float, string etc\). Every domain value should instead be represented by a domain primitive. 

Advantages:

* All the validation for each domain primitive is in one place.
* No validation is needed in the state handling business logic – if the value exists, it is automatically valid. This makes the business logic a lot cleaner.
* There is also less risk of mixing up parameters in method calls – _Quantity_ and _DeliveryDays_ is better than _int_ and _int_. Furthermore, bugs of the type where a negative amount of books is ordered become impossible.

#### ENTITIES

**Limited operations.** Don’t have methods that can do more than what is allowed by the business logic. For example, if an order entity has a field for whether it is paid or not, it should default to _false_ at creation. Then there should only be a method that is called for example _markPaid_, that sets the field to _true_. This is better than a _setPaid_ method that could set the value to either _true_ or _false_, since that would make it possible to go from paid to not paid, which is not valid \(if this is the business rule\).

**Not sharing mutable objects.** For the entity to be able to uphold its internal constraints, it must not leak references to internal objects. Suppose there is a _Customer_ object, and it has a reference to a _CreditScore_ object. Even if the variable holding the _CreditScore_ reference is final, the _CreditScore_ object can still be modified by anybody holding a reference to it. To be sure the _CreditScore_ is set once and never modified, a copy of it must be stored in the _Customer_ object \(and the reference to that must never be exposed outside the _Customer_ object\). The same problem exists for collections, like lists. They are mutable by default. If an internal collection needs to be exposed outside the entity, a copy of it \(using for example a copy constructor\) should be returned.

Sometimes, if there are complicated consistency rules that must be upheld, it can be good to define a method called e.g. _checkInvariants\(\)_. This can be called at the end of regular mutating methods, to make sure the entity is still internally consistent.

