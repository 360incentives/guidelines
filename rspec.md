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
- In unit tests, avoid touching the database, objects should be built in memory
- Never mock the object under test, only objects which it talks to
- Use shared examples carefully and only where it makes sense. [Duplication is
  far cheaper than the wrong
  abstraction](https://www.sandimetz.com/blog/2016/1/20/the-wrong-abstraction)

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

- Extract hardcoded values to well-named `let` blocks, in the same manner as
  constants

  ```ruby
  # bad
  expect(1 + 2).to eq(3)

  # good
  subject { 1 + 2 }
  let(:expected_result) { 3 }

  it { is_expected.to eq(expected_result)}
  ```
