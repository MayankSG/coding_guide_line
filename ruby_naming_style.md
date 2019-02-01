## Ruby Naming

**1.** Always use Name identifiers in English.

```ruby
# bad - identifier using non-ascii characters
заплата = 1_000

# bad - identifier is a Bulgarian word, written with Latin letters (instead of Cyrillic)
zaplata = 1_000

# good
salary = 1_000
```

**2.** Use snake_case for symbols, methods and variables.

```ruby
# bad
:'some symbol'
:SomeSymbol
:someSymbol

# good
:some_symbol

--------------------------------------

#bad
someVar = 5

#good
some_var = 5

--------------------------------------

#bad
def someMethod
  # some code
end

def SomeMethod
  # some code
end

#good
def some_method
  # some code
end
```

**3.** Do not separate numbers from letters on symbols, methods and variables.

```ruby
# bad
:some_sym_1

some_var_1 = 1

var_10  = 10

def some_method_1
  # some code
end

# good
:some_sym1

some_var1 = 1

var10    = 10

def some_method1
  # some code
end
```

**4.** Use CamelCase for classes and modules

```ruby
# bad
class Someclass
  # some code
end

class Some_Class
  # some code
end

class SomeXml
  # some code
end

class XmlSomething
  # some code
end

# good
class SomeClass
  # some code
end

class SomeXML
  # some code
end

class XMLSomething
  # some code
end
```

**5.** Use snake_case for naming files.

```ruby
# hello_world.rb
```

**6.** Use snake_case for naming directories.

```ruby
# lib/hello_world/hello_world.rb
```

**7.** Aim to have just a single class/module per source file. Name the file name as the class/module, but replacing CamelCase with snake_case.

```ruby
#file name e.g
# hello_world.rb

# class name in file
class HelloWord
end
```

**8.** Use SCREAMING_SNAKE_CASE for other constants.

```ruby
# bad
SomeConst = 5

# good
SOME_CONST = 5
```

**9.** he names of predicate methods (methods that return a boolean value) should end in a question mark.

```ruby
  def tall?
    true
  end
```

**10.** Avoid prefixing predicate methods with the auxiliary verbs such as is, does, or can.

```ruby
# bad
  def is_tall?
    true
  end

  def can_play_basketball?
    false
  end


# good
  def tall?
    true
  end

  def basketball_player?
    false
  end
```
**11.** You should end method with an exclamation mark(!) if there exists a safe version of that dangerous method.

"Method end with ! mark are dangerous method(e.g. update!)"

"Method without ! mark are safe method(e.g. update)"

```ruby
# bad - there is no matching 'safe' method
class Person
  def update!
  end
end

# good
class Person
  def update
  end
end

# good
# in this example we use !(update!) method only when its safe(update) method exist

class Person
  def update!
  end

  def update
  end
end
```
