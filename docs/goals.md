# Goals
Starkey is a language defined by many very opinionated goals. This part of the docs is my chance to express how and why Starkey will implement these goals.

### Inspiration from Rust
Rust is by far my best and favorite language. While languages like Python and Lua are fun to use, Rust is such and unfathomably well-designed language and every piece fits together like clockwork. I might be getting a little carried away with my praise, but I really like Rust and there's no denying it inspired Starkey quite a bit.

### Starkey's Core Values
Starkey is a static & strongly typed programming language with a focus on data science, immutability, and safe alteration of data. As a language, Starkey follows a few major principles:

> 1. Data that is mutated or changed must be done so obviously.
> 2. Types assigned to data may never change and can only be temporarily type cast.
> 3. Control flow, including loops and recursive functions, should be easy to follow.
> 4. Anonymous functions should be well-implemented to allow for their streamlined use.
> 5. Operators and symbols should only be used for one purpose. Multiple actions should not share a single operator.
> 6. While whitespace and tabs are not tokenized, scope and context should be easy to follow and portray via the use of symbols and keywords.

## Immutability by Default
While on the surface this seems out of touch, it is designed to fit the core values of Starkey. One of the things that defines Starkey is it's deliberate approach to data manipulation. 

> Need to create a variable? Awesome. Let's do that.

> Need to mutate a variable? Well... Let's make sure you're sure what you're doing first.

Starkey variables require a `mut` at definition if you want to change them later.

```none
new mut i = 0
```

This prevents you from mutating a variable that you didn't mean to change, or causing unexpected behavior as the result of a function. Ultimately this is baby steps for the principles of Starkey. The real deal comes with `mut` keywords in function parameters and whatnot.

## The Mutate Operator
The mutate operator is another aspect of the immutability by default and deliberate manipulation of data. By using a different entire operator for setting versus modifying variables, we can effectively eliminate any scenario involving the accidental change of a variable.

```none
new mut message = "Hello, World!"
$ message
message -> "Hello, Mom!"
$ message
```
Output:
```none
Hello, World!
Hello, Mom!
```