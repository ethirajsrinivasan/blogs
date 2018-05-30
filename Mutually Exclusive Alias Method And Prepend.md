## Mutually Exclusive Alias Method And Prepend

Module#alias_method was one of the earliest solution to overwrite a method in ruby.With an alias method in place you can redefine a method to anything you want and call the old method using the alias_method

Let us consider the following example
```ruby
class Person
 def greeting
  puts "Hello"
 end
end

class Person
  def greeting_with_expression
    puts "Hi,Hello"
    greeting_without_expression
  end

  alias_method :greeting_without_expression, :greeting
  alias_method :greeting, :greeting_with_expression
end
```

In the example above Person#greeting is overridden using alias method.'greeting_without_expression' calls the actual greeting method.

With the advent of Module#prepend in Ruby 2.0 It allows you override a method in a class with a method from a module, and still access the class’s implementation with super
```ruby
module Greeting
  def greeting
    puts "!!! Hello"
    super
  end
end

Person.prepend(Greeting)
```

Greeting module is prepended to Person.And now when you call
```ruby
Person.new.greeting
```

You will end up with
```shell
SystemStackError: stack level too deep
```

What happened on the background is that Greeting#greeting was called at first then it calls its super_method(Person#greeting_with_expression). At the end of execution of greeting_with_expression it calls the greeting_without_expression method which again calls Greeting#greeting due to method lookup.This creates a deadlock situation and results in stack level too deep error.

Learning from the above example is that you will not be able to combine Moudle#prepend and Module#alias_method.You can follow only one of these method if you want to overwrite a method at multiple levels.

Lookup for alias method and prepend when you come across stack level too deep error next time.Happy Coding !!!
