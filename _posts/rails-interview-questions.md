---
layout: post
title: "Ruby on Rails Inteview Question"
description: Creating static site with Jekyll
categories: jekyll web-development
image:
status: draft
---

### What's is different beetwen `:includes` and `:join`?

`.joins` will just joins the tables and brings selected fields in return. If you call associations on joins query result, it will fire database queries again

`:includes` will eager load the included associations and add them in memory, and `:includes` loads all the included tables attributes. If you call associations on include query result, it will not fire any queries


### Explain polymorphics in Rails!

https://guides.rubyonrails.org/association_basics.html#polymorphic-associations


Polymorphic associations allow a model to belong to more than one other model through a single association.

***

The advantage of using polymorphism here is that it allows you to create a single comment model, rather than separate models for each one (PhotoComment model, ArticleComment model, etc.) into Commentable, imageable, etc

For example, you might have a picture model that belongs to either an employee model or a product model. Here's how this could be declared:

```rb
class Picture < ApplicationRecord
  belongs_to :imageable, polymorphic: true
end

class Employee < ApplicationRecord
  has_many :pictures, as: :imageable
end

class Product < ApplicationRecord
  has_many :pictures, as: :imageable
end
```

You can think of a polymorphic belongs_to declaration as setting up an interface that any other model can use. From an instance of the Employee model, you can retrieve a collection of pictures: @employee.pictures.

Similarly, you can retrieve @product.pictures.

### How to create many to many in Rails?

Rails offers two different ways to declare a many-to-many relationship between models.

The first way is to use `has_and_belongs_to_many`, which allows you to make the association directly.

```rb
class Assembly < ApplicationRecord
  has_and_belongs_to_many :parts
end

class Part < ApplicationRecord
  has_and_belongs_to_many :assemblies
end
```

The second way to declare a many-to-many relationship is to use has_many `:through`. This makes the association indirectly, through a join model.

```rb
class Assembly < ApplicationRecord
  has_many :manifests
  has_many :parts, through: :manifests
end

class Manifest < ApplicationRecord
  belongs_to :assembly
  belongs_to :part
end

class Part < ApplicationRecord
  has_many :manifests
  has_many :assemblies, through: :manifests
end
```

### Whats the different beetwen  `include` and `extend` module?

The difference between include and extend is that `include` is for adding methods only to an instance of a class and `extend` is for adding methods to the class but not to it's instance.

```rb
# Creating a module contains a method
module Hello
  def say_hi
    puts 'Hello'
  end
end

class Human
  include Hello

  # only can access say_hi methods
  # with the instance of the class.
end

class Computer
  extend Hello

  # only can access geek methods
  # with the class definition.
end

# object access
Human.new.say_hi

# class access
Computer.say_hi

# NoMethodError: undefined  method
# `say_hi' for Human:Class
Human.say_hi
```

### Explain local variable, instance variable, class variable and global variable.

Local variables e.g `age = 10` only can accessed in same method.

Instance variables e.g `@age = 10` are available across methods for any specified instance or object.

Class variable e.g `@@age = 10` belongs to the class and it is available across any specified instance or object on same class. Class variables are not available across classes.

Global variable e.g `$age = 10`. If you want to have a single variable, which is available across classes. Its scope is global, means it can be accessed from anywhere in a program.
