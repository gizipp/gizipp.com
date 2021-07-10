---
layout: page
title: RSpec Cheatsheets
permalink: /cheatsheets/rspec
date: 2021-07-10
last_modified_at: 2021-07-10
---

#### Expectation

```rb
expect(actual).to     eq(expected)      # passes if actual == expected
expect(actual).to     eql(expected)     # passes if actual.eql?(expected)
expect(actual).not_to eql(not_expected) # passes if not(actual.eql?(expected))

expect(actual).to     be(expected)      # passes if actual.equal?(expected)
expect(actual).to     equal(expected)   # passes if actual.equal?(expected)

expect(actual).to be >  expected
expect(actual).to be >= expected
expect(actual).to be <= expected
expect(actual).to be <  expected
expect(actual).to be_within(delta).of(expected)

expect(actual).to match(/expression/)

expect(actual).to be_an_instance_of(expected) # passes if actual.class == expected
expect(actual).to be_a(expected)              # passes if actual.kind_of?(expected)
expect(actual).to be_an(expected)             # an alias for be_a
expect(actual).to be_a_kind_of(expected)      # another alias

expect(actual).to be_truthy   # passes if actual is truthy (not nil or false)
expect(actual).to be true     # passes if actual == true
expect(actual).to be_falsy    # passes if actual is falsy (nil or false)
expect(actual).to be false    # passes if actual == false
expect(actual).to be_nil      # passes if actual is nil
expect(actual).to_not be_nil  # passes if actual is not nil

expect { ... }.to raise_error
expect { ... }.to raise_error(ErrorClass)
expect { ... }.to raise_error("message")
expect { ... }.to raise_error(ErrorClass, "message")

expect { ... }.to throw_symbol
expect { ... }.to throw_symbol(:symbol)
expect { ... }.to throw_symbol(:symbol, 'value')

expect([1, 2, 3]).to eq([1, 2, 3])            # Order dependent equality check
expect([1, 2, 3]).to include(1)               # Exact ordering, partial collection matches
expect([1, 2, 3]).to include(2, 3)            #
expect([1, 2, 3]).to start_with(1)            # As above, but from the start of the collection
expect([1, 2, 3]).to start_with(1, 2)         #
expect([1, 2, 3]).to end_with(3)              # As above but from the end of the collection
expect([1, 2, 3]).to end_with(2, 3)           #
expect({:a => 'b'}).to include(:a => 'b')     # Matching within hashes
expect("this string").to include("is str")    # Matching within strings
expect("this string").to start_with("this")   #
expect("this string").to end_with("ring")     #
expect([1, 2, 3]).to contain_exactly(2, 3, 1) # Order independent matches
expect([1, 2, 3]).to match_array([3, 2, 1])   #

expect(alphabet).to start_with("a").and end_with("z")
expect(stoplight.color).to eq("red").or eq("green").or eq("yellow")
```

#### Describe

```rb
describe '#associations' do
  it { is_expected.to have_many(:payment_proofs) }
  it { is_expected.to belong_to(:user) }
end
```

#### Context

```rb
describe '#validations' do
  it { should validate_presence_of(:full_name) }
  it { should validate_presence_of(:email) }
  it { should validate_presence_of(:password) }
  it { should validate_length_of(:password).is_at_least(8) }

  context ".email" do
    subject { build(:user) }

    it { should allow_value("satrio+test@wagginton.com").for(:email) }
    it { should_not allow_value("satrio.wagginton.com").for(:email) }
    it { should_not allow_value("satrio@waggintoncom").for(:email) }
  end
```
