---
layout: page
title: Ruby Cheatsheets
permalink: /cheatsheets/ruby
date: 2021-07-07
last_modified_at: 2021-07-08
---

### Map

- Map transfrom data from Arrays, Hashes & Ranges.
- Map returns a new array with the results. To to change the original array use `map!`

#### Map basic

```rb
["a", "b", "c"].map { |string| string.upcase }
```

#### Map With Index

```rb
["a", "b", "c"].map.with_index { |char, index| [ch, index] }
```

#### Map With Index NOT Start from 0

```rb
["a", "b", "c"].map.with_index(1) { |char, index| [ch, index] }
```

#### Map Shorthand &:

```rb
["1", "2", "3"].map(&:to_i)

["cat", "dog", "fish"].map(&:class)
```

#### Map from Hash

```rb
hash = {
  name: 'Matz',
  language: 'Ruby'
}

hash.map { |key, value| [key, value.to_sym] }.to_h
```

### Uniq

```rb
[1, 1, 1, 1, 2, 3].uniq # => [1, 2, 3]
```

Use `uniq!` to change array.

### nil? and friends

#### nil?

- Only return `true` only for `nil`

```ruby
nil.nil? # => true
false.nil? # => false
0.nil? # => false
"".nil? # => false
```

#### empty?

```rb
[].empty? # => true
"".empty? # => true
" ".empty? # => false
```

#### blank?

```rb
nil.blank? # => true
false.blank? # => true
[].blank? # => true
{}.blank? # => true
"".blank? # => true
"       ".blank? # => true
5.blank? # => false
0.blank? # => false
```

#### present?

Negatives of `blank?`

```rb
!blank? == present? #=> true
```

### Misc

#### Random number

```rb
[*1..100].sample
```

#### Meta Programming Define Methods

```rb
class MyClass
  1.upto(1000) do |n|
    define_method :"method_#{n}" do
      puts "I am method #{n}!"
    end
  end
end
```

####

```rb
def get_bearer_token
  pattern = /^Bearer/
  header = request.authorization
  header = request.env['Authorization'] if header.blank?
  header.gsub(pattern, '').strip if header && header.match(pattern)
end
```
