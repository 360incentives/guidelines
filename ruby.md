# Ruby

For the most part, we try to adhere to [this community driven style
guide](https://github.com/bbatsov/ruby-style-guide); however, we have a few
modifications and extras.

## Table of Contents

- [Source Code Layout](#source-code-layout)
- [Classes & Modules](#classes--modules)
- [Strings](#strings)

## Source Code Layout

Everything from [here](https://github.com/bbatsov/ruby-style-guide#source-code-layout)
and these points.

- <a name="avoid-locals"></a>Avoid using local variables where possible; prefer
  extracting a method <sup>[[link](#avoid-locals)]</sup>

  ```ruby
  # bad
  class Foo
    def something
      foo = 'foo'
      bar = 'bar'

      puts foo + bar
    end
  end

  # good
  class Foo
    def something
      puts foo + bar
    end

    private

    def foo
      'foo'
    end

    def bar
      'bar'
    end
  end
  ```

- <a name="conditional-assignment"></a>Avoid assigning a variable in a conditional;
  prefer extraction of a well-named method <sup>[[link](#conditional-assignment)]</sup>

  ```ruby
  # bad
  if something?
    some_variable = 'something'
  else
    some_variable = 'nothing'
  end

  puts some_variable

  # better
  some_variable = if something?
                    'something'
                  else
                    'nothing'
                  end

  puts some_variable

  # best
  def some_variable_value
    if something?
      'something'
    else
      'nothing'
    end
  end

  puts some_variable_value
  ```
- <a name="method-chaining"></a>Avoid method chaining if intermediate values
  have their own meaning; prefer extracting a method
  <sup>[[link](#method-chaining)]</sup>

  ```ruby
  # ok
  [1, 2, '3', nil].select { |val| val.is_a?(Integer) }.map { |val| val + 1 }

  # better
  def raw_array
    [1, 2, '3', nil]
  end

  def numbers_array
    raw_array.select { |val| val.is_a?(Fixnum) }
  end

  def incremented_array
    numbers_array.map { |val| val + 1 }
  end

  incremented_array
  ```

- <a name="method-chaining-multi"></a>When method chaining requires multiple
  lines, put each chained method on it's own line with a leading `.`
  <sup>[[link](#method-chaining-multi)]</sup>

  ```ruby
  # ok
  one.two.three
    .four

  # better
  one
    .two
    .three
    .four
  ```

- <a name="multiline"></a>When a method call or array definition require
  multiple lines, put each parameter on it's own line.
  <sup>[[link](#multiline)]</sup>

  ```ruby
  # bad
  some_method(:foo, :bar,
              :baz)

  [:foo, :bar,
   :baz]

  # good
  some_method(
    :foo,
    :bar,
    :baz
  )

  [
    :foo,
    :bar,
    :baz
  ]
  ```

## Classes & Modules

Everything from [here](https://github.com/bbatsov/ruby-style-guide#classes--modules)
and these points.

- <a name="private-attr"></a>Prefer private attr_readers over explicit use of
  instance variables <sup>[[link](#private-attr)]</sup>

  ```ruby
  class Foo
    def initialize(bar)
      @bar = bar
    end

    # bad
    def something
      puts @bar
    end

    # good (usually the following lines would be above #initialize)
    attr_reader :bar
    private :bar

    def something
      puts bar
    end
  end
  ```

## Strings

Everything from [here](https://github.com/bbatsov/ruby-style-guide#strings)
and these points.

- <a name="single-quotes"></a>Single quotes by default. Option A in [the style
  guide](https://github.com/bbatsov/ruby-style-guide#consistent-string-literals)
  <sup>[[link](#single-quotes)]</sup>
