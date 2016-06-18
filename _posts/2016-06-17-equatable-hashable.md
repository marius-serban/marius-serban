---
layout: post
title: A purpose for Equatable and Hashable in Swift
---

There is [a lot](#for-more-in-depth-reading) written on the subject of object equality and object hashes covering it in depth, but I wanted to summarise it and to frame it in the context of Swift. This article is not focused on *how* to implement these protocols, but more focused on *why* you would want to implement them.

### Equatable

Equatable expresses **under what conditions** type instances are considered to be equivalent (or equal) through the lens of a certain business logic. There is no correct or incorrect way of implementing equality, it all depends on how you want to define it in your code. For example, when implementing equality for a data model type, you can take into consideration all properties or only a subset of them.

There are two reasons why you may want to implement `Equatable`:

1. you can easily compare type instances using the `==` and `!=` operators throughout your code
2. some collections like `Array` provide searching functions like `indexOf` or `contains` which can take an element directly as a parameter instead of closure as they would otherwise do

### Hashable

Now if you want to store your items in collections such as `Set`s or `Dictionary`s, it's a requirement that you implement `Hashable`. This is purely for performance optimization reasons. From a logical point of view, equality alone is enough to have items organised in sets. However, because the underlying implementations of `Set` and `Dictionary` use hash tables in order to make lookups faster, one must conform to `Hashable`.

Interestingly enough, `Set`s will admit two elements that have the same `hashValue`, provided that the elements are not equal as defined when conforming to the `Equatable` protocol. However, this strips away the performance gain of hash lookups.

There is a strong connection between the two protocols in Swift:

- they are both useful when dealing with elements as part of collections
- they both refer to the concept of equality through the lens of a domain specific logic
- one defines the equality concept more thoroughly while the other optimizes it for use in hash tables

<div class="message">
  When conforming to <em>Hashable</em> remember that any two <strong>equal</strong> type instances <strong>must</strong> have the same <em>hashValue</em>. You won't get any compiler warnings about breaking this rule, so it's up to you to enforce it. 
</div>

-----

#### For more in depth reading:
1. [https://developer.apple.com/reference/swift/equatable](https://developer.apple.com/reference/swift/equatable)
2. [https://developer.apple.com/reference/swift/hashable](https://developer.apple.com/reference/swift/hashable)
3. [http://nshipster.com/equality/](http://nshipster.com/equality/)
4. [https://msdn.microsoft.com/en-us/library/dd183755.aspx](https://msdn.microsoft.com/en-us/library/dd183755.aspx)
5. [http://www.javaworld.com/article/2072762/java-app-dev/object-equality.html?page=2](http://www.javaworld.com/article/2072762/java-app-dev/object-equality.html?page=2)
6. [https://www.ibm.com/developerworks/library/j-jtp05273/](https://www.ibm.com/developerworks/library/j-jtp05273/)
