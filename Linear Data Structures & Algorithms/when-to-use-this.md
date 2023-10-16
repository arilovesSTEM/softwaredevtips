---
tags:
  - linear-data-structures-algorithms
  - tips-and-tricks
---
# When do I use `this`?

the `this` keyword is very helpful for telling java (and other OOP languages that use this keyword) explicitly when you are referring to a class or instance variable, rather than an identically named one within a method's scope.

take for example, a `Person` with a constructor which takes in a `name` and `age`:

```java
public class Person {

	private String name;
	private int age;
	private int lucky_number;

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
		lucky_number = name.length;
	}

}
```

we use `this` here because there are two `name`s defined in this scope. the `name` parameter in our constructor, and the `name` variable already defined at the top of our class, and vice-versa for `age`.

here we explicitly tells java "please set the *instance* `name` and `age` to the `name` and `age` given by the *constructor*." doing this the other way around would result in the constructor parameters being set to the (ostensibly empty) values currently held by the instance- nothing would change!

note that we don't use `this.lucky_number`, as there is only one `lucky_number` in the scope of this constructor. this would not be the case for a `set_lucky_number()` method which takes another `lucky_number` as a parameter.

# what if i name my parameters differently?

then you don't need to use `this`! here's the same example with this principle applied:

```java
public class Person {

	private String name;
	private int age;
	private int lucky_number;

	public Person(String new_name, int new_age) {
		name = new_name;
		age = new_age;
		lucky_number = name.length;
	}

}
```

in this example, `this` isn't used anywhere in the code, because every single variable has a unique name. people tend to prefer the former method however, as it's simpler than deciding a fancy, unqiue name for every parameter.