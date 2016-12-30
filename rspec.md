# RSpec

In general, we try to adhere to the [BetterSpecs
guidelines](http://betterspecs.org/). This document is a few extensions and key
points from that.

## Key points

- Make liberal use of [`describe`](http://betterspecs.org/#describe) and
  [`context`](http://betterspecs.org/#contexts) blocks
- Only use the [`expect`](http://betterspecs.org/#expect) and
  `is_expected.to` syntax
- Create only the [data you need](http://betterspecs.org/#data)
- Avoid talking to the database if possible. Use mocks and non-persisted objects

## Syntax

- `let` blocks should use multiple line `{ }` proc syntax. `{ }` syntax is less
  distracting than `do end`

  ```ruby
  # Bad
  let(:foo) do
    some_very_long_instantiation
  end

  # Good
  let(:bar) {
    some_very_long_instantiation
  }

  # Also good
  let(:baz) { 'baz' }
  ```

- Use `{ }` blocks for expectations

  ```ruby
  # bad
  expect do
    something
  end.to change(Something, :else)

  # good
  expect { something }.to change(Something, :else)

  # also good
  expect {
    something
  }.to change(Something, :else)
  ```

- Use `do end` for `before` blocks

  ```ruby
  # bad
  before {
    do_something
  }

  # ok
  before { do_something }

  # good
  before do
    do_something
  end
  ```

  The difference between `before` and `let`/`expect` is due to how it reads.
  e.g `let(:foo) { :bar }` reads like 'let foo be equal to bar' whereas
  `before do something; end` reads like 'before the test, do this thing'.
