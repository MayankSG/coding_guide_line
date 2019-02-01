## Classes & Modules
**1.** Use a proper and consistent structure in your class definitions.
Means which code should come first and then, like in below example  extend/include should be added top, then inner class, then constant and so on.

```ruby
class Person
  # extend and include go first
  extend SomeModule
  include AnotherModule

  # inner classes
  CustomError = Class.new(StandardError)

  # constants are next
  SOME_CONSTANT = 20

  # afterwards we have attribute macros
  attr_reader :name

  # followed by other macros (if any)
  validates :name

  # public class methods are next in line
  def self.some_method
  end

  # initialization goes between class methods and other instance methods
  def initialize
  end

  # followed by other public instance methods
  def some_method
  end

  # protected and private methods are grouped near the end
  protected

  def some_protected_method
  end

  private

  def some_private_method
  end
end
```

**2.** Split multiple mixins into separate statements.
```ruby
# bad
class Person
  include Foo, Bar
end

# good
class Person
  # multiple mixins go in separate statements
  include Foo
  include Bar
end
```

**3.** Don't write nested  classes within classes. Try to have such nested classes each in their own file in a folder named like the containing class.

```ruby
# bad

# foo.rb
class Foo
  class Bar
    # 30 methods inside
  end

  class Car
    # 20 methods inside
  end

  # 30 methods inside
end

# good

# foo.rb
class Foo
  # 30 methods inside
end

# foo/bar.rb
class Foo
  class Bar
    # 30 methods inside
  end
end

# foo/car.rb
class Foo
  class Car
    # 20 methods inside
  end
end
```

**4.** Prefer modules instead classes if have only class methods. Classes should be used only when it makes sense to create instances out of them.

```ruby
# bad
class SomeClass
  def self.some_method
    # body omitted
  end

  def self.some_other_method
    # body omitted
  end
end

# good
module SomeModule
  module_function

  def some_method
    # body omitted
  end

  def some_other_method
    # body omitted
  end
end
```

**5.** Use of module_function instead extend self when you want to turn a module's instance methods into class methods.

```ruby
# bad
module Utilities
  extend self

  def parse_something(string)
    # do stuff here
  end

  def other_utility_method(number, string)
    # do some more stuff
  end
end

# good
module Utilities
  module_function

  def parse_something(string)
    # do stuff here
  end

  def other_utility_method(number, string)
    # do some more stuff
  end
end
```

**6.** Always supply a proper to_s method for classes that represent domain objects.
In below example 'to_s' method represent Person's object .e.g @person.to_s will return full name with first_name, last_name

```ruby
class Person
  attr_reader :first_name, :last_name

  def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end

  def to_s
    "#{first_name} #{last_name}"
  end
end
```

**7.** Use the attr family of functions to define trivial accessors or mutators.

```ruby
# bad
class Person
  def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end

  def first_name
    @first_name
  end

  def last_name
    @last_name
  end
end

# good
class Person
  attr_reader :first_name, :last_name

  def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end
end
```

**8.** For accessors and mutators, avoid prefixing method names with get_ and set_.

```ruby
# bad
class Person
  def get_name
    "#{@first_name} #{@last_name}"
  end

  def set_name(name)
    @first_name, @last_name = name.split(' ')
  end
end

# good
class Person
  def name
    "#{@first_name} #{@last_name}"
  end

  def name=(name)
    @first_name, @last_name = name.split(' ')
  end
end
```

**9.** Use attr_reader and attr_accessor instead of attr.

```ruby
# bad - creates a single attribute accessor (deprecated in Ruby 1.9)
attr :something, true
attr :one, :two, :three # behaves as attr_reader

# good
attr_accessor :something
attr_reader :one, :two, :three
```

**10.**  Prefer duck-typing over inheritance
Duck Typing means an object type is defined by what it can do. Duck Typing refers to the tendency of Ruby to be less concerned with the class of an object and more concerned with what methods can be called on it and what operations can be performed on it.
```ruby
# bad
class Animal
  # abstract method
  def speak
  end
end

# extend superclass
class Duck < Animal
  def speak
    puts 'Quack! Quack'
  end
end

# extend superclass
class Dog < Animal
  def speak
    puts 'Bau! Bau!'
  end
end

# good
class Duck
  def speak
    puts 'Quack! Quack'
  end
end

class Dog
  def speak
    puts 'Bau! Bau!'
  end
end
```

**11.** Avoid the usage of class (@@) variables due to their "nasty" behavior in inheritance.

```ruby
class Parent
  @@class_var = 'parent'

  def self.print_class_var
    puts @@class_var
  end
end

class Child < Parent
  @@class_var = 'child'
end

Parent.print_class_var # => will print 'child'

# As you can see all the classes in a class hierarchy actually share one class variable. Class instance variables should usually be preferred over class variables.
```

**12.** Assign proper visibility levels to methods (private, protected) in accordance with their intended usage.

```ruby
Class SomeClass
  def public_method
    # some code
  end

  protected

  def protected_method
    # some code
  end

  private

  def private_method
    # some code
  end
end
```

**13.** Use def self.method to define class methods.

```ruby
class TestClass
  # bad
  def TestClass.some_method
    # body omitted
  end

  # good
  def self.some_other_method
    # body omitted
  end

  # Also possible and convenient when you
  # have to define many class methods.
  class << self
    def first_method
      # body omitted
    end

    def second_method_etc
      # body omitted
    end
  end
end
```

**14.** When class (or module) methods call other such methods, omit the use of a leading self or own name followed by a . when calling other such methods.

```ruby
class TestClass
  # bad -- more work when class renamed/method moved
  def self.call(param1, param2)
    TestClass.new(param1).call(param2)
  end

  # bad -- more verbose than necessary
  def self.call(param1, param2)
    self.new(param1).call(param2)
  end

  # good
  def self.call(param1, param2)
    new(param1).call(param2)
  end

  # ...other methods...
end
```


## String

**1.** Use string interpolation over concatenation.

```ruby
#bad
string_1 + '<' + string2 + '>'

#good
"#{string_1} < #{string2} >"
```

**2.** Assign single quotes to the string which does not have interpolation.

```ruby
#bad
"string 1"

#good
'string 1'
```

**3.** Use %w for the string arrays and %i for the symbol arrays.

```ruby
STATUS_MAP = %w(open closed draft paid)
SYMBOL_MAP = %i(symbol1 symbol2 symbol3)
```
