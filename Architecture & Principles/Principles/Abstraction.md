
## Definition

The `abstract` keyword is used for classes and methods:

- **Abstract class:** is a restricted class that cannot be used to create objects (to access it, it must be inherited from another class).
  
- **Abstract method:** can only be used in an abstract class, and it does not have a body. The body is provided by the derived class (inherited from).

The principle of abstraction is another specialization of the principle of separation of concerns  ([[Separation of Concerns]].) Following the principle of abstraction implies separating the behavior of software components from their implementation. It requires learning to look at software and software components from two points of view: what it does, and how it does it.

Failure to separate behavior from implementation is a common cause of unnecessary coupling. For example, it is common in recursive algorithms to introduce extra parameters to make the recursion work. When this is done, the recursion should be called through a non-recursive shell that provides the proper initial values for the extra parameters. Otherwise, the caller must deal with a more complex behavior that requires specifying the extra parameters. If the implementation is later converted to a non-recursive algorithm then the client code will also need to be changed.

## Simple way to explain it:

> An abstract class or method serves as a blueprint or template for the other classes or methods to follow. It provides a structure and defines some common behaviors or characteristics that other classes or methods can inherit.

## Explanation Like I am 5 yo

Imagine you have a big box full of colorful LEGO blocks. Each block has its own shape, size, and color. Now, let's say you want to build a really cool spaceship using those blocks.

But wait, there's a problem! You don't know how to build a spaceship because it's too complicated. So what can you do? You can start by using some special blocks that are already designed to look like parts of a spaceship, such as a cockpit, wings, and engines. These special blocks are like abstract ideas.

When you use these special blocks, you don't need to worry about how they are made or how they work internally. You just know that if you put them together in a certain way, they will look like a spaceship. This way, you can focus on building the spaceship without getting confused by all the details.

In software development, abstraction is a similar concept. It means using pre-built, simplified pieces of code that represent complex ideas or tasks. These pieces of code are like the special LEGO blocks that help you build a spaceship. By using abstraction, developers can focus on solving problems and creating software without getting overwhelmed by all the technical details behind the scenes. It makes things easier to understand and work with, just like using the special blocks makes it easier to build a spaceship.
