# Method Arguments

## Objectives

1. Understand the purpose of supplying a method with arguments.
2. Define methods that accept arguments.
3. Use a method's arguments within the body of the method.
4. Understand how method scope works in Ruby.


### 1. Understanding Arguments

Imagine needing to build a method that counts from 1 to 10. We could code something like this, using a cool ruby loop `upto`.

```ruby
def count_from_one_to_ten
  1.upto(10) do |i| # Opens the loop for all number between 1 and 10
    # Don't worry about the line above, just know that the next line 
    # executes once for every number between 1 and 10, with i being
    # equal to the current number or iteration on every loop (1,2,3,etc).    
    puts i

  end # This ends the loop's block.
end # Ends the method
```

This method will print out every number between `1` and `10`. Try it out, open an IRB session by running `irb` from your command line. Once you're in your IRB shell, paste in the code:

```ruby
def count_from_one_to_ten
  1.upto(10) do |i|
    puts i
  end
end
```

```bash
$ irb
001:0 > def count_from_one_to_ten
002:1 >   1.upto(10) do |i|
003:2>      puts i
004:2 >   end
005:1 > end
=> :count_from_one_to_ten
```

You've now defined the method. Notice that it did not execute. Type the following into IRB to execute your method: `count_from_one_to_ten`.

```bash
006:0 > count_from_one_to_ten
```

You should see:

```bash
1
2
3
4
5
6
7
8
9
10
=> 1
```

As amazing as this method is, it's still pretty literal. It hard-codes, or directly specifies the number to count to and the number to count from. If we wanted to build a method that counts from one to twenty, we'd have to re-implement the majority of the original logic from `count_from_one_to_ten`:

```ruby
def count_from_one_to_twenty
  1.upto(20) do |i|
    puts i
  end
end
```

Notice the only things that changed are the method name and the number `20` in the body of the method. It's as though that information should be specifiable or configurable when you call the method, otherwise we'd have to build every permutation of the method. Ideally, we would want our method to be more dynamic, more abstract, only providing the details of the number to count to on execution, not on definition.

Good news, that's exactly what method arguments (also called parameters) are for:

```ruby
def count_from_one_to(end_of_range)
  1.upto(end_of_range) do |i|
    puts i
  end
end

count_from_one_to(3)
# > 1
# > 2
# > 3

count_from_one_to(100)
# > 1
# > 2
# > 3
# > ...
# > 100
```

Let's dig into how to add arguments to our methods.

### 2. Defining Method Arguments

To add arguments to a method, you specify them in the method signature––the line that starts with `def`. Simply add parentheses after the name of the method and create a placeholder name for your argument. 

For example, if I want to write a method called `greeting` that accepts an argument of a person's name, I would do it like this: 

```ruby
    #method name      #argument
def greeting_a_person(name)
  "Hello " + name
end
``` 

Arguments create new local variables that can be used within the method. When you name an argument, all you are defining is what bareword you want to use to access that data, just like when you create a variable. Arguments follow the same rules as local variables: they can be any word that starts with a lowercase letter and they should be as descriptive of the data as possible.

Let's look at an example of a method that takes in an argument and sets that argument as the value of a local variable: 

```ruby
def arguments_and_local_variables(name)
  person = name
  "Hello " + person
end

arguments_and_local_variables("George R.R. Martin")
  #=> "Hello George R.R. Martin"

```

Let's revisit our earlier example, `count_from_one_to`:


```ruby
  # method name       argument list
def count_from_one_to(end_of_range)
  1.upto(end_of_range) do |i|
    puts i
  end
end
```

Now, we've defined our method to take in an argument and that argument will inform the `.upto` loop when to stop counting. Now it's your turn!

%%%

#### Code Challenge I: Defining Methods with and Argument

Define a method, `plus_one_machine` that takes in an argument of a number and adds `1` to that number. Then, invoke the method with an argument of `9`. 

~~~ruby

# define your method here!

# invoke your method here!

~~~solution

def plus_one_machine(num)
  num + 1
end

plus_one_machine(9)

~~~validation

assert_equal(response, 10)

~~~

%%%

#### Defining Methods with Multiple Arguments

You can define a method to accept as many arguments as you want. In the example above, as dynamic as our `count_from_one_to` method is now that it accepts an `end_of_range` at which to stop counting, it still only allows us to start at `1`. What if we wanted to change that as well? We could code the method to accept two arguments:

```ruby
  # method name   first_argument  second_argument
def count_from_to(start_of_range, end_of_range)
  # ... We'll see this implementation below, can you guess it?
end

count_from_to(1,3)
# > 1
# > 2
# > 3

count_from_to(5,25)
# > 5
# > 6
# > 7
# > ...
# > 25
```

To accept multiple arguments, simply separate the barewords in the argument list with commas. Give it a shot:

%%%

#### Code Challenge II: Defining Methods with Multiple Arguments

Define a method, `sum_machine` that takes in two arguments, two integers, and adds those integers together. Then, invoke the method with an arguments of `9` and `1`. 

~~~ruby

# define your method here!

# invoke your method here!

~~~solution

def sum_machine(num1, num2)
  num1 + num2
end

sum_machine(9, 1)

~~~validation

assert_equal(response, 10)

~~~

%%%


#### Required Arguments

Once you define arguments for a method, they become required when you invoke or call the method. If you define a method that accepts a singular argument, when you call that method, you must supply a value for that argument, otherwise, you get an `ArgumentError`. Here's an example:

```ruby
def count_from_one_to(end_range)
  # ... etc
end

count_from_one_to() # I explicitly call the method without a value for the argument end_range
# > ArgumentError: wrong number of arguments (0 for 1)
```

In Ruby, all arguments are required when you invoke the method. You can't define a method to accept an argument and call the method without that argument. Additionally, a method defined to accept one argument will raise an error if called with more than one argument.

```ruby
def count_from_one_to(end_range)
  # ... etc
end

count_from_one_to(100, 5) # The method accepts 1 argument and I supplied 2.
# > ArgumentError: wrong number of arguments (2 for 1)
```

By default, all arguments defined in a method are required in order to correctly invoke (or "call", or "execute") that method. If you build the method to behave a certain way and have certain expectations about data, you must follow those rules.

#### Optional Arguments with Default Values

Often we want our methods to accept arguments but also have a default value for that argument. Imagine our use case of the method `count_from_to`. We designed that method to be very dynamic, you can supply it with two (2) values, a number to start counting from and a number to stop counting at. That means that if we want to count from `1` to `100`, we need to invoke `count_from_to(1,100)`, if we want to count from `1` to `10`, we need to invoke `count_from_to(1,10)`, from `1` to `1000`, we need to invoke `count_from_to(1,1000)`. It's probably a reasonable assumption that unless specified, we want to start counting from `1`. However, given the method definition of:

```ruby
def count_from_to(start_of_range, end_of_range)
  # ...
end
```

We would constantly need to provide a value for the argument `start_of_range`, even if it was obvious and common, like the value `1`. The method is defined to require two arguments no matter what. Invoking it as `count_from_to(100)` would raise `ArgumentError: wrong number of arguments (1 for 2)`.

How can we supply a default value for an argument thereby making it optional upon method invocation? Simple. In the argument list, assign a **default value**.

```ruby
#                 assigning a default value
def count_from_to(start_of_range = 1, end_of_range)
  # ...
end
```

In our argument list, `(start_of_range = 1, end_of_range)`, we simply assign the argument `start_of_range` a default value of `1`. By doing so, if the method is invoked with only one supplied argument, ruby will assume the value of the other argument to be its default, `1`:

```ruby
#                 assigning a default value
def count_from_to(start_of_range = 1, end_of_range)
  # ...
end

count_from_to(100)
# Will count from 1 to 100

count_from_to(10)
# Will count from 1 to 10

count_from_to(10, 100)
# Will count from 10 to 100

count_from_to
# Will raise an ArgumentError: wrong number of arguments (0 for 1..2)
```

With default arguments our once simple machine becomes profoundly useful and abstract:

```ruby
#                 1 arg with default, 2 required arg
def count_from_to(start_of_range = 1, end_of_range)
  start_of_range.upto(end_range) do |i|
    puts i
  end
end
```

We can now invoke this method with `count_from_to(100)` to count from `1` to `100`. Invoking this method with only one (1) argument forces ruby to assume that you, the programmer, are supplying a value for the undefined argument `end_of_range` and not for `start_of_range` which has a default value.

You can also invoke it with `count_from_to(100,1000)` to count from `100` to `1000`, now supplying values for both arguments. 

That's the power of abstraction when combining methods with arguments. You can build a machine, a method, that changes it's behavior when it is invoked, even containing defaults for certain values.

Default arguments are easy to add, you simply assign them a default value with `=` ("equals") in the argument list to inherit if the argument is not supplied upon evocation. There's no limit to the amount of arguments that you can make default.

```ruby
#                 1 arg with default, 2 arg with default
def count_from_to(start_of_range = 1, end_of_range = 100)
  start_of_range.upto(end_range) do |i|
    puts i
  end
end

count_from_to
# > 1
# > 2
# > 3
# > ...
# > 100

count_from_to(10)
# > 1
# > 2
# > 3
# > ...
# > 10

count_from_to(10,20)
# > 10
# > 11
# > 12
# > ...
# > 20
```

Method arguments, both required and optional, make methods powerfully abstract and dynamic machines that are easy to build yet very flexible and adaptable to different situations and requirements. Get used to defining methods with required and default arguments and calling them correctly. Let's give default arguments a try right now: 

%%%

#### Code Challenge III: Defining Methods with Default Arguments

Define a method, `plus_one_or_whatever` that takes in a required argument of a number and a default argument of `1`. The method should return the sum of the two arguments. Then call your method with **only one argument**, `9`. It will return `10` because of your default argument!

~~~ruby

# define your method here

# invoke it here!

~~~solution

def plus_one_or_whatever(num1, num2=1)
  num1 + num2
end

plus_one_or_whatever(9)

~~~validation

assert_equal(response, 10)

~~~

%%%

Method arguments, both required and optional, make methods powerfully abstract and dynamic machines that are easy to build yet are very flexible and adaptable to different situations and requirements. Get used to defining methods with required and default arguments and calling them correctly.

### 3. Using Arguments in Methods

Now that we know how to define a method with arguments, either required, optional, or both, we should quickly talk about using those arguments, that data, within the method. Consider the simpler example of `count_from_one_to(end_range)`. This is a simple method that will count up to the number supplied to the method upon invocation.

```ruby
def count_from_one_to(end_range)
  1.upto(end_range) do |i|
    puts i
  end
end

# > count_from_one_to(5) # Will count from 1 to 5
# > count_from_one_to(50) # Will count from 1 to 50
```

When we define a method with arguments we are defining a bareword that we can use to reference the actual value supplied to the method upon invocation. We built a method that will count from `1` to any specified number. When we actually call that method, we need a word, an abstraction, a variable, that we can refer to that idea of "any specified number" as. That's an argument.

```ruby
def count_from_one_to(end_of_range)
  1.upto(end_of_range) do |i|
    puts i
  end
end
```

When we build that method we might ask ourselves, "up to what number might this method count?". The answer is "any number supplied, it doesn't matter." That's what makes the method abstract, the detail of what number it counts up to is hidden until the method is actually invoked: `count_from_one_to(10)`. Only then do we know that the method counts up to `10`. The value of `end_of_range` is only supplied upon evocation.

When we define a method argument, we can assume that a valid value for that argument is provided upon execution of the method. The point of the argument is to make some aspect of the methods procedure abstract. Compare the original method `count_from_one_to_ten` to our dynamic method with an argument for the end of the counting.

```ruby
def count_from_one_to_ten
  1.upto(10) do |i|
    puts i
  end
end
count_from_one_to_ten #> Counts from 1 to 10

def count_from_one_to(end_of_range)
  1.upto(end_of_range) do |i|
    puts i
  end
end

count_from_one_to(100) #> Counts from 1 to 100
count_from_one_to(50)  #> Counts from 1 to 50
```

The bareword we use as the argument's name in the method signature becomes a local variable within the method. Through that variable we can reference the value of the argument supplied at invocation.

With the code above, when we say: `count_from_one_to(100)`, the value of the argument `end_of_range` is `100`. During the particular runtime invoked by `count_from_one_to(100)`, any reference to `end_of_range` will have the value of `100`, allowing the method to behave as intended.

Similarly, when we say: `count_from_one_to(50)`, the value of the argument `end_of_range` is `50`. So During that particular invocation, `count_from_one_to(50)` the value of the argument `end_of_range` is `50`, so the method behaves differently.

Method arguments simply create local variables for you to refer to the value used when the method is actually invoked. Imagine the following examples:

```ruby
def say_hello_ten_times
  10.times do 
    puts "Hello"
  end
end
say_hello_ten_times
# > Will say "Hello" 10 times.
```

The first thing we should abstract from this very literal method is the phrase that is being said ten (10) times. Let's add an argument to the method and use it within the method body.

```ruby
def say_ten_times(phrase) 
  # phrase is a local variable referencing the value passed on evocation.
  10.times do 
    puts phrase 
  end
end
say_ten_times("You're awesome") # phrase now equals "You're awesome"
# > Will say "You're awesome" ten times
say_ten_times("You're programming") # phrase now equals "You're programming"
# > Will say "You're programming" ten times
say_ten_times # > Will raise an ArgumentError, 0 for 1.
```

In the first example we abstract the phrase said ten times out of the literal method name and into a dynamic required method argument. But if we invoke the method without a phrase to say, we get an `ArgumentError`; after all, how can we say something ten times when we don't even know what we're saying?

Let's make it more dynamic with a default phrase.

```ruby
def say_ten_times(phrase = "Hello")
  10.times do 
    puts phrase
  end
end

say_ten_times("I <3 Ruby")
# > Will say "I <3 Ruby" ten times
say_ten_times
# > Will say "Hello", the default value for phrase, ten times.
```

Awesome, no more errors and we have a default value for the variable `phrase` within the method.

Let's take it a step further; in addition to abstracting the phrase repeated, yet providing a default phrase "Hello", let's abstract the amount of times the phase is repeated. So we add an additional argument to the method.

```ruby
def say_x_times(phrase = "Hello", x)
# Below, the once literal value 10 is replaced with the dynamic value of x
# supplied to the method when it is evoked.
  x.times do 
    puts phrase
  end
end
```

Notice where we once used the literal value `10` in the method body, we now replace that with a reference to `x`. The value for `x` will be supplied when the method is invoked, as demonstrated quickly below:

```ruby
def say_x_times(phrase = "Hello", x)
  x.times do 
    puts phrase
  end
end

say_x_times(10)
# > Will print "Hello" 10 times.
# Since x is required and phrase is optional, ruby assumes that 10 is the
# value for the required argument, x, and not the optional argument, phrase.

say_x_times("I can code", 10)
# > Will print "I can code" 10 times.

say_x_times
# > ArgumentError: wrong number of arguments (0 for 1..2)
# Calling the method with no arguments raises an error. 
```

One last thing before we go...

### 4. Method Scope

Methods in ruby create their own scope. **Any local variable created outside of a method will be unavailable inside of a method. In addition, local variables created inside of a method 'fall out of scope' once you're outside the method.**

Think of a method as a castle. The `def` and `end` keywords are like the gates that keep out the barbarian hordes, dragons, etc. Let's take a look: 

```ruby 
evil_monster = "Bowser"

def princess_peaches_castle
  puts "#{evil_monster} is trying to kidnap Princess Peach!"
end
```

We've defined the variable `evil_monster` *outside* of the method, `princess_peaches_castle`. Then, we try to call on the `evil_monster` variable inside that method. Watch what happens when we invoke the method: 

```ruby
princess_peaches_castle
  #=> NameError: undefined local variable or method `evil_monster' for main:Object
```

The `evil_monster` variable is out of scope for this method. The method can't access it **unless we pass it in as an argument**. 

If we define our method to accept an argument, we can pass our variable into the method and the method will be able to operate on/use that variable. Let's take a look: 

```ruby
evil_monster = "Bowser"

def princess_peaches_castle(bad_guy)
  puts "#{bad_guy} is trying to kidnap Princess Peach!"
end

princess_peaches_castle(evil_monster)
  |––> "Bowser is trying to kidnap Princess Peach!"

```

And now Mario can start his adventure. 

So far, we've seen that variables defined outside of methods are not available inside methods (unless we pass them in as arguments). This works the other way around as well: variables defined inside of methods are not available outside of those methods. Let's take a look. If we define the following method to include a local variable:

```ruby
def practicing_method_scope
  im_trapped_in_the_method = "You can't access me outside this method!"
end
```

Trying to access that variable elsewhere in our program, *outside of this method*, will raise the following error:

```ruby
im_trapped_in_the_method
  #=> NameError: undefined local variable or method `im_trapped_in_the_method' for main:Object
```

### Conclusion

That's really it. We've covered how to define a method, how to add arguments, how to make arguments optional by providing a default value, how to use the arguments within the method *and* how method scoping works in Ruby.

