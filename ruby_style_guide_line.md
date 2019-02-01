## Make your code readable and Beautiful

"Indentation: Is number of blank spaces to align the start and end of block of code, or statement. e.g."

Here block start with 'def' and finish by 'end' in same alignment, it's child 'do_something' align inner with some blank space. And so on with inner blocks in it

```ruby
def some_method
  do_something

  do_something1 do
    do_something2
  end
end
```

**1.** Use two spaces per indentation level (aka soft tabs). No hard tabs.

```ruby
# bad - four spaces
def some_method
    do_something
end

# good
def some_method
  do_something
end
```

**2.** 100-line classes.
Lines per file or class should be not more then 100. It make the code more readable and arranged. also help to write DRY code.

**3.** Five lines per method
Methods should not be more than 5 line.

```ruby
def validate_actor
  if actor_type == 'Group'
    user_must_belong_to_group
  elsif actor_type == 'User'
    user_must_be_the_same_as_actor
  end
end
```

**4.** Three method arguments
The arguments in method should be one or two. In any case three not more than three. Instead of you can use options.

```ruby
#bad
def foo(a, b, c, d, e)
  puts [a, b, c]
end

#good
options = {a: 1, b: 2, c: 3, d: 4, e: 5}

def foo(options={})
  puts [options[:a], options[:b], options[:c]]
end
```

**5.** Don't use ; to separate statements and expressions. As a corollary—use one expression per line.

```ruby
# bad
puts 'foobar'; # superfluous semicolon
puts 'foo'; puts 'bar' # two expressions on the same line

# good
puts 'foobar'
puts 'foo'
puts 'bar'
puts 'foo', 'bar' # this applies to puts in particular
```

**6.** Prefer a single-line format for class definitions with no body.

```ruby
# bad
class FooError < StandardError
end

# okish
class FooError < StandardError; end

# good
FooError = Class.new(StandardError)
```

**7.** Avoid single-line methods

```ruby
# bad
def too_much; something; something_else; end

# good
def some_method
  body
end

Only exception to the rule If method have empty-body.
# good
def no_op; end
```

**8.** Use spaces around operators, after commas, colons and semicolons. Whitespace might be (mostly) irrelevant to the Ruby interpreter, but its proper use is the key to writing easily readable code.

```ruby
sum = 1 + 2
a, b = 1, 2
class FooError < StandardError; end
There are a few exceptions. One is the exponent operator:

# bad
e = M * c ** 2

# good
e = M * c**2
Another exception is the slash in rational literals:

--------------------------------------

# bad
o_scale = 1 / 48r

# good
o_scale = 1/48r
Another exception is the safe navigation operator:

--------------------------------------

# bad
foo &. bar
foo &.bar
foo&. bar

# good
foo&.bar
```

**9.** No spaces after (, [ or before ], ). Use spaces around { and before }.

```ruby
# bad
some( arg ).other
[ 1, 2, 3 ].each{|e| puts e}

# good
some(arg).other
[1, 2, 3].each { |e| puts e }

--------------------------------------

{ and } deserve a bit of clarification, since they are used for block and hash literals, as well as string interpolation
# good - space after { and before }
{ one: 1, two: 2 }

# good - no space after { and before }
{one: 1, two: 2}
With interpolated expressions, there should be no padded-spacing inside the braces.

--------------------------------------

# bad
"From: #{ user.first_name }, #{ user.last_name }"

# good
"From: #{user.first_name}, #{user.last_name}"
```

**10.** No space after !

```ruby
# bad
! something

# good
!something
```

**11.** No space inside range literals.

```ruby
# bad
1 .. 3
'a' ... 'z'

# good
1..3
'a'...'z'
```

**12.** while use when-case, when should indent same indentation as case.

```ruby
# bad
case
  when song.name == 'Misty'
    puts 'Not again!'
  when song.duration > 120
    puts 'Too long!'
  when Time.now.hour > 21
    puts "It's too late"
  else
    song.play
end

# good
case
when song.name == 'Misty'
  puts 'Not again!'
when song.duration > 120
  puts 'Too long!'
when Time.now.hour > 21
  puts "It's too late"
else
  song.play
end
```

**13.** When we assign the result of a conditional expression to a variable, use same alignment as condition start. means alignment should start from where condition start.

```ruby
# bad - pretty convoluted
kind = case year
when 1850..1889 then 'Blues'
end

result = if some_cond
  calc_something
end

# good - it's apparent what's going on
kind = case year
       when 1850..1889 then 'Blues'
       end

result = if some_cond
           calc_something
         end

# good (and a bit more width efficient)
kind =
  case year
  when 1850..1889 then 'Blues'
  end

result =
  if some_cond
    calc_something
  end
```

**14.** Use empty lines between method definitions and also to break up methods into logical paragraphs internally.

```ruby
#bad
def some_method
  data = initialize(options)

  data.manipulate!

  data.result
end

#good
def some_method
  result
end
```

**15.** Don't use several empty lines in a between methods or statements.

```ruby
# bad - It has two empty lines.
some_method


some_method

# good
some_method

some_method
```

**16.**  Use empty lines around access modifiers.

```ruby
# bad
class Foo
  attr_reader :foo
  def foo
    # do something...
  end
end

# good
class Foo
  attr_reader :foo

  def foo
    # do something...
  end
end
```

**17.** Don't use empty lines around method, class, module, block bodies.

```ruby
# bad
class Foo

  def foo

    begin

      do_something do

        something

      end

    rescue

      something

    end

  end

end

# good
class Foo
  def foo
    begin
      do_something do
        something
      end
    rescue
      something
    end
  end
end
```

**18.** Avoid comma after the last parameter in a method call, especially when the parameters are not on separate lines.

```ruby
# bad
some_method(size, count, color, )

# good
some_method(size, count, color)

## Use spaces around the = operator when assigning default values to method parameters:

--------------------------------------

# bad
def some_method(arg1=:default, arg2=nil, arg3=[])
  # do something...
end

# good
def some_method(arg1 = :default, arg2 = nil, arg3 = [])
  # do something...
end
```

**19.** Avoid line continuation \ (This use for continuous line break) where not required. In practice, avoid using line continuations for anything but string concatenation.

```ruby
# bad
result = 1 - \
         2

# good (but still ugly as hell)
result = 1 \
         - 2

long_string = 'First part of the long string' \
              ' and second part of the long string'
```

**20.** Align the parameters of a method call if they span more than one line. When aligning parameters is not appropriate due to line-length constraints, single indent for the lines after the first is also acceptable.

```ruby
# starting point (line is too long)
def send_mail(source)
  Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
end

# bad (double indent)
def send_mail(source)
  Mailer.deliver(
      to: 'bob@example.com',
      from: 'us@example.com',
      subject: 'Important message',
      body: source.text)
end

# good
def send_mail(source)
  Mailer.deliver(to: 'bob@example.com',
                 from: 'us@example.com',
                 subject: 'Important message',
                 body: source.text)
end

# good (normal indent)
def send_mail(source)
  Mailer.deliver(
    to: 'bob@example.com',
    from: 'us@example.com',
    subject: 'Important message',
    body: source.text
  )
end
```

**21.** Align the elements of array literals spanning multiple lines.

```ruby
# bad - single indent
menu_item = %w[Spam Spam Spam Spam Spam Spam Spam Spam
  Baked beans Spam Spam Spam Spam Spam]

# good
menu_item = %w[
  Spam Spam Spam Spam Spam Spam Spam Spam
  Baked beans Spam Spam Spam Spam Spam
]

# good
menu_item =
  %w[Spam Spam Spam Spam Spam Spam Spam Spam
     Baked beans Spam Spam Spam Spam Spam]
```

**22.** Add underscores to large numeric literals to improve their readability.

```ruby
# bad - how many 0s are there?
num = 1000000

# good - much easier to parse for the human brain
num = 1_000_000
```

**23.** Prefer lowercase letters for numeric literal prefixes. 0o for octal, 0x for hexadecimal and 0b for binary. Do not use 0d prefix for decimal literals.

```ruby
# bad
num = 01234
num = 0O1234
num = 0X12AB

# good - easier to separate digits from the prefix
num = 0o1234
num = 0x12AB
num = 1234
```

**24.** Limit lines to 80 characters.

```ruby
if you are using Sublime Editor. set property in setting

"wrap_width": 80
"word_wrap": true
```

**25.** Avoid trailing white-space.

```ruby
if you are using Sublime Editor. set property in setting
"trim_trailing_white_space_on_save": false
```

**26.** End each file with a newline.

```ruby
if you are using Sublime Editor. set property in setting
  "ensure_newline_at_eof_on_save": true
```

**27.** Don't use block comments.

```ruby
# bad
=begin
comment line
another comment line
=end

# good
# comment line
# another comment line
```


**28.** Use UTF-8 as the source file encoding.
