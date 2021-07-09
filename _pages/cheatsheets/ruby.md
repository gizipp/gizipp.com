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

### nil?

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
