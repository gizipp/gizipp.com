---
layout: page
title: Ruby Cheatsheets
permalink: /cheatsheets/ruby
date: 2021-07-07
last_modified_at: 2021-07-08
---

## Map

- Map transfrom data from Arrays, Hashes & Ranges.
- Map returns a new array with the results. To to change the original array use `map!`

### Map basic

```rb
["a", "b", "c"].map { |string| string.upcase }
```

### Map With Index

```rb
["a", "b", "c"].map.with_index { |char, index| [ch, index] }
```

### Map With Index NOT Start from 0

```rb
["a", "b", "c"].map.with_index(1) { |char, index| [ch, index] }
```

### Map Shorthand &:

```rb
["1", "2", "3"].map(&:to_i)

["cat", "dog", "fish"].map(&:class)
```

### Map from Hash

```rb
hash = {
  name: 'Matz',
  language: 'Ruby'
}

hash.map { |key, value| [key, value.to_sym] }.to_h
```
