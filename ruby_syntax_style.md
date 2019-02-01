## Make Syntax Proper

**1.** Use :: only to reference constants (this includes classes and modules) and constructors (like Array() or Nokogiri::HTML()).

**2.** Do not use :: for regular method invocation.
```ruby
# bad
SomeClass::some_method
some_object::some_method

# good
SomeClass.some_method
some_object.some_method
SomeModule::SomeClass::SOME_CONST
SomeModule::SomeClass()
```

**3.** Do not use :: to define class methods.

```ruby
# bad
class Foo
  def self::some_method
  end
end

# good
class Foo
  def self.some_method
  end
end
```

**4.** Use def with parentheses when there are parameters. Omit the parentheses when the method doesn't accept any parameters.

```ruby
# bad
def some_method()
  # body omitted
end

# good
def some_method
  # body omitted
end

--------------------------------------

# bad
def some_method_with_parameters param1, param2
  # body omitted
end

# good
def some_method_with_parameters(param1, param2)
  # body omitted
end
```

**5.** Use parentheses around the arguments of method invocations.

```ruby
# bad
x = Math.sin y

# good
x = Math.sin(y)

--------------------------------------

# bad
array.delete e

# good
array.delete(e)

--------------------------------------

# bad
temperance = Person.new 'Temperance', 30

# good
temperance = Person.new('Temperance', 30)
```

**6.** Don't use parentheses for Method calls with no arguments.

```ruby
# bad
Kernel.exit!()
2.even?()
fork()
'test'.upcase()

# good
Kernel.exit!
2.even?
fork
'test'.upcase
```

**7.** Don't use parentheses for Methods that are part of an internal DSL (e.g., Rake, Rails, RSpec).

```ruby
# bad
validates(:name, presence: true)

# good
validates :name, presence: true
```

**8.** Don't use parentheses for Methods that have "keyword" status in Ruby.

```ruby
class Person
  # bad
  attr_reader(:name, :age)

  # good
  attr_reader :name, :age

  # body omitted
end
```

**9.** Define optional arguments at the end of the list of arguments
here a=1 and b = 2 are optional argument

```ruby
# bad
def some_method(a = 1, b = 2, c, d)
  puts "#{a}, #{b}, #{c}, #{d}"
end

some_method('w', 'x') # => '1, 2, w, x'
some_method('w', 'x', 'y') # => 'w, 2, x, y'
some_method('w', 'x', 'y', 'z') # => 'w, x, y, z'

# good
def some_method(c, d, a = 1, b = 2)
  puts "#{a}, #{b}, #{c}, #{d}"
end

some_method('w', 'x') # => '1, 2, w, x'
some_method('w', 'x', 'y') # => 'y, 2, w, x'
some_method('w', 'x', 'y', 'z') # => 'y, z, w, x'
```

**10.** Use keyword arguments when passing Boolean argument to a method.

```ruby
# bad
def some_method(bar = false)
  puts bar
end

# bad - common hack before keyword args were introduced
def some_method(options = {})
  bar = options.fetch(:bar, false)
  puts bar
end

# good
def some_method(bar: false)
  puts bar
end

some_method            # => false
some_method(bar: true) # => true
```

**11.** Prefer keyword arguments over optional arguments.

```ruby
# bad
def some_method(a, b = 5, c = 1)
  # body omitted
end

# good
def some_method(a, b: 5, c: 1)
  # body omitted
end
```

**12.** Use keyword arguments instead of option hashes.

```ruby
# bad
def some_method(options = {})
  bar = options.fetch(:bar, false)
  puts bar
end

# good
def some_method(bar: false)
  puts bar
end
```

**13.** Avoid the use of parallel assignment for defining variables. Parallel assignment is allowed when it is the return of a method call, used with the splat operator, or when used to swap variable assignment.

```ruby
# bad
a, b, c, d = 'foo', 'bar', 'baz', 'foobar'

# good
a = 'foo'
b = 'bar'
c = 'baz'
d = 'foobar'

# good - swapping variable assignment
# Swapping variable assignment is a special case because it will allow you to
# swap the values that are assigned to each variable.
a = 'foo'
b = 'bar'

a, b = b, a
puts a # => 'bar'
puts b # => 'foo'

# good - method return
def multi_return
  [1, 2]
end

first, second = multi_return

# good - use with splat
first, *list = [1, 2, 3, 4] # first => 1, list => [2, 3, 4]

hello_array = *'Hello' # => ["Hello"]

a = *(1..3) # => [1, 2, 3]
```

**14.** Do not use for, user each instead.

```ruby
arr = [1, 2, 3]

# bad
for elem in arr do
  puts elem
end

# note that elem is accessible outside of the for loop
elem # => 3

# good
arr.each { |elem| puts elem }
```

**15.** Do not use then for multi-line if/unless.

```ruby
# bad
if some_condition then
  # body omitted
end

# good
if some_condition
  # body omitted
end
```

**16.** Always put the condition on the same line as the if/unless in a multi-line conditional.

```ruby
# bad
if
  some_condition
  do_something_else
end

# good
if some_condition
  do_something
end
```

**17.** Use the ternary operator(?:) over if/then/else/end . It's more common and obviously more concise.

```ruby
# bad
result = if some_condition then something else something_else end

# good
result = some_condition ? something : something_else
```

**18.** Ternary operators must not be nested. Prefer if/else constructs in these cases.

```ruby
# bad
some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

# good
if some_condition
  nested_condition ? nested_something : nested_something_else
else
  something_else
end
```

**19.** Do not use if x; .... Use the ternary operator instead.

```ruby
# bad
result = if some_condition; something else something_else end

# good
result = some_condition ? something : something_else
```

**20.**  if and case are expressions which return a result. So we don't need to use extra variable.

```ruby
# bad
if condition
  result = x
else
  result = y
end

# good
result =
  if condition
    x
  else
    y
  end
```

**21.** Use ! instead of not.

```ruby
# bad - parentheses are required because of op precedence
x = (not something)

# good
x = !something
```

**22.** Avoid the use of !! .

```ruby
# bad
x = 'test'
# obscure nil check
if !!x
  # body omitted
end

# good
x = 'test'
if x
  # body omitted
end
```

**23.** The `and` and `or` keywords are banned, always use && and || instead.

```ruby
# bad
# boolean expression
ok = got_needed_arguments and arguments_are_valid

# control flow
document.save or raise("Failed to save document!")

# good
# boolean expression
ok = got_needed_arguments && arguments_are_valid

# control flow
raise("Failed to save document!") unless document.save

# ok
# control flow
document.save || raise("Failed to save document!")
```

**24.** Favor modifier if/unless usage when you have a single-line body. Another good alternative is the usage of control flow &&/|| .

```ruby
# bad
if some_condition
  do_something
end

# good
do_something if some_condition

# another good option
some_condition && do_something
```

**25.** Avoid modifier if/unless usage at the end of a non-trivial multi-line block.

```ruby
# bad
10.times do
  # multi-line body omitted
end if some_condition

# good
if some_condition
  10.times do
    # multi-line body omitted
  end
end
```

**26.**  Avoid nested modifier if/unless/while/until usage. Favor &&/|| if appropriate.

```ruby
# bad
do_something if other_condition if some_condition

# good
do_something if some_condition && other_condition

--------------------------------------

## Favor unless over if for negative conditions (or control flow ||)

# bad
do_something if !some_condition

# bad
do_something if not some_condition

# good
do_something unless some_condition

# another good option
some_condition || do_something
```

**27.** Do not use unless with else. Rewrite these with the positive case first.

```ruby
# bad
unless success?
  puts 'failure'
else
  puts 'success'
end

# good
if success?
  puts 'success'
else
  puts 'failure'
end


## Don't use parentheses around the condition of a control expression.

# bad
if (x > 10)
  # body omitted
end

# good
if x > 10
  # body omitted
end
```

**28.** Do not use while/until condition do for multi-line while/until.

```ruby
# bad
while x > 5 do
  # body omitted
end

# good
while x > 5
  # body omitted
end

--------------------------------------

#bad
until x > 5 do
  # body omitted
end

#good
until x > 5
  # body omitted
end
```

**29.** When single line body, usage while/until .

```ruby
# bad
while some_condition
  do_something
end

# good
do_something while some_condition
```

**30.** Use until over while for negative conditions.

```ruby
# bad
do_something while !some_condition

# good
do_something until some_condition
```

**31.** Use Kernel#loop instead of while/until when you need an infinite loop.

```ruby
# bad
while true
  do_something
end

until false
  do_something
end

# good
loop do
  do_something
end
```

**32.** Don't use the outer braces around an implicit options hash.

```ruby
# bad
user.set({ name: 'John', age: 45, permissions: { read: true } })

# good
user.set(name: 'John', age: 45, permissions: { read: true })
```

**33.** Remove both the outer braces and parentheses for methods that are part of an internal DSL.
```rubby
class Person < ActiveRecord::Base
  # bad
  validates(:name, { presence: true, length: { within: 1..10 } })

  # good
  validates :name, presence: true, length: { within: 1..10 }
end
```

**34.** Use the proc invocation shorthand when the invoked method is the only operation of a block.

```ruby
# bad
names.map { |name| name.upcase }

# good
names.map(&:upcase)
```

**35.** Prefer {...} over do...end for single-line blocks.

```ruby
names = %w[Bozhidar Steve Sarah]

# bad
names.each do |name|
  puts name
end

# good
names.each { |name| puts name }
```

**36.** Avoid using {...} for multi-line blocks.

```ruby
names = %w[Bozhidar Steve Sarah]

# bad
names.each { |name| puts name puts "My name is #{name}" }

# good
names.each do |name|
  puts name
  puts "My name is #{name}"
end
```

**37.** Avoid return where not required for flow of control.

```ruby
# bad
def some_method(some_arr)
  return some_arr.size
end

# good
def some_method(some_arr)
  some_arr.size
end
```

**38.** Avoid self where not required. (It is only required when calling a self write accessor, methods named after reserved words, or overloadable operators).

```ruby
# bad
def ready?
  if self.last_reviewed_at > self.last_updated_at
    self.worker.update(self.content, self.options)
    self.status = :in_progress
  end
  self.status == :verified
end

# good
def ready?
  if last_reviewed_at > last_updated_at
    worker.update(content, options)
    self.status = :in_progress
  end
  status == :verified
end
```

**38.** Don't use the return value of = (an assignment) in conditional expressions.

```ruby
# bad (+ a warning)
if v = array.grep(/foo/)
  do_something(v)
  # some code
end

# good
v = array.grep(/foo/)
if v
  do_something(v)
  # some code
end
```

**39.** Use shorthand self assignment operators whenever applicable.

```ruby
# bad
x = x + y
x = x * y
x = x**y
x = x / y
x = x || y
x = x && y

# good
x += y
x *= y
x **= y
x /= y
x ||= y
x &&= y
```

**40.** Use ||= to initialize variables only if they're not already initialized.

```ruby
# bad
name = name ? name : 'Bozhidar'

# bad
name = 'Bozhidar' unless name

# good - set name to 'Bozhidar', only if it's nil or false
name ||= 'Bozhidar'
```

**41.** Don't use ||= to initialize boolean variables.

```ruby
# bad - would set enabled to true even if it was false
enabled ||= true

# good
enabled = true if enabled.nil?
```

**42.** Use &&= to preprocess variables that may or may not exist. Using &&= will change the value only if it exists, removing the need to check its existence with if.

```ruby
# bad
if something
  something = something.downcase
end

# bad
something = something ? something.downcase : nil

# ok
something = something.downcase if something

# good
something = something && something.downcase

# better
something &&= something.downcase
```

**43.** Avoid explicit use of the case equality operator ===.

```ruby
# bad
Array === something
(1..100) === 7
/something/ === some_string

# good
something.is_a?(Array)
(1..100).include?(7)
some_string.match?(/something/)
```

**44.** Do not use eql? when using == will do.

```ruby
# bad - eql? is the same as == for strings
'ruby'.eql? some_str

# good
'ruby' == some_str
1.0.eql? x # eql? makes sense here if want to differentiate between Integer and Float 1
```

**45.** Do not put a space between a method name and the opening parenthesis.

```ruby
# bad
f (3 + 2) + 1

# good
f(3 + 2) + 1
```

**46.** Do not use nested method definitions.

```ruby
# bad
def foo(x)
  def bar(y)
    # body omitted
  end

  bar(x)
end

# good - the same as the previous, but no bar redefinition on every foo call
def bar(y)
  # body omitted
end

def foo(x)
  bar(x)
end
```

**47.** Use the new lambda literal syntax for single line body blocks. Use the lambda method for multi-line blocks.

```ruby
# bad
l = lambda { |a, b| a + b }
l.call(1, 2)

# correct, but looks extremely awkward
l = ->(a, b) do
  tmp = a * 7
  tmp * b / 50
end

# good
l = ->(a, b) { a + b }
l.call(1, 2)

l = lambda do |a, b|
  tmp = a * 7
  tmp * b / 50
end
```

**48.** Prefer proc over Proc.new .

```ruby
# bad
p = Proc.new { |n| puts n }

# good
p = proc { |n| puts n }
```

**49.** Use of Array#join over Array#* with a string argument.

```ruby
# bad
%w[one two three] * ', '
# => 'one, two, three'

# good
%w[one two three].join(', ')
# => 'one, two, three'
```

**50.** Use ranges or Comparable#between? instead of complex comparison logic when possible.

```ruby
# bad
do_something if x >= 1000 && x <= 2000

# good
do_something if (1000..2000).include?(x)

# good
do_something if x.between?(1000, 2000)
```

**51.** Use of predicate methods to explicit comparisons with ==. Numeric comparisons are OK.

```ruby
# bad
if x % 2 == 0
end

# good
if x.even?
end

--------------------------------------

#bad
if x % 2 == 1
end

#good
if x.odd?
end

--------------------------------------

#bad
if x == nil
end

#good
if x.nil?
end

--------------------------------------

#bad
if x == 0
end

#good
if x.zero?
end

```

**52.** Don't need to  check non-nil unless you're dealing with boolean values.

```ruby
# bad
do_something if !something.nil?
do_something if something != nil

# good
do_something if something

# good - dealing with a boolean
def value_set?
  !@some_boolean.nil?
end
```

**53.** Avoid the use of BEGIN blocks.

```ruby
#bad
begin
  @splines.reticulate
end

#good
@splines.reticulate

```

**54.** Prefer next in loops instead of conditional blocks.

```ruby
# bad
[0, 1, 2, 3].each do |item|
  if item > 1
    puts item
  end
end

# good
[0, 1, 2, 3].each do |item|
  next unless item > 1
  puts item
end
```

**55.** Prefer map over collect, find over detect, select over find_all, reduce over inject and size over length.

```ruby
#good
[1, 2, 3].collect { |n| n == 2 ? n : nil }

#better
[1, 2, 3].map { |n| n == 2 ? n : nil }

--------------------------------------

#good
[1, 2, 3].detect { |n| n == 2 ? n : nil }

#better
[1, 2, 3].find { |n| n == 2 ? n : nil }

--------------------------------------

#good
[1, 2, 3].find_all { |n| n == 2 ? n : nil }

#better
[1, 2, 3].select { |n| n == 2 ? n : nil }

--------------------------------------
#good
[1, 2, 3].inject([]) do |memo, n|
  memo << n if n == 2
  memo
end

#better
[1, 2, 3].reduce([]) do |memo, n|
  memo << n if n == 2
  memo
end

--------------------------------------
#bad
[1, 2, 3].length

#good
[1, 2, 3].size

```

**56.** Don't use count as a substitute for size. For Enumerable objects other than Array it will iterate the entire collection in order to determine its size.


```ruby
# bad
some_hash.count

# good
some_hash.size
```

**57.** Use flat_map instead of map + flatten.

```ruby
user.songs = ['a', ['b','c']]

# bad
all_songs = users.map(&:songs).flatten.uniq #=> ['a', 'b','c']

# good
all_songs = users.flat_map(&:songs).uniq #=> ['a', 'b','c']
```

**58.** Prefer reverse_each to reverse.each because some classes that include Enumerable will provide an efficient implementation.

```ruby
# bad
array.reverse.each { ... }

# good
array.reverse_each { ... }
```
