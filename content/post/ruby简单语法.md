---
title: "Ruby简单语法"
date: 2022-12-29T09:45:12+08:00
categories:
  - Note
  - daily
tags:
  - Ruby
---

## Ruby的简单语法

1. class

```ruby

class User
  def initialize(name)
    @name = name
  end
  def hi(target)
    p "Hi #{target}, I'm #{@name}"
  end
end

user1 = User.new('Lily')
user1.hi('frank')

```

定义属性的get set

```ruby

class Person
  def name
    @name
  end
  def name= xx
    @name = xx
  end
end

p = Person.new


p p.name
p.name= '11adsf'
p p.name

```

可以使用attr_accessor来简化

```ruby
class Person
  attr_accessor :name, :age
  attr_accessor :email
end

p = Person.new


p p.name
p.name= '11adsf'
p p.name
```

2. 数组

```ruby

even_numbers = []

# 相当于js中的map
[1,2,3,4,5,6].each do |num|
 if num.even?
 even_numbers << num
 end
end

p even_numbers

# 简写
[1,2,3,4,5,6].each do |num|
 even_numbers << num if num.even?
end

p even_numbers

# 相当于js中的filter
even_numbers = [1,2,3,4,5,6].select do |num|
 num.even?
end

# 简写
even_numbers = [1,2,3,4,5,6].select{ |num| num.even? }
# 或
even_numbers = [1,2,3,4,5,6].select(&:even?)
# 或
even_numbers = (1..6).select(&:even?)

# 就地排序
even_numbers = (1..6).to_a
even_numbers.select!(&:even?)
p even_numbers

```
