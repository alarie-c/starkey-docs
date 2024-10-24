# Goals
Starkey is a language defined by many very opinionated goals. This part of the docs is my chance to express how and why Starkey will implement these goals.

## Inspiration from Rust
Rust is by far my best and favorite language. While languages like Python and Lua are fun to use, Rust is such and unfathomably well-designed language and every piece fits together like clockwork. I might be getting a little carried away with my praise, but I really like Rust and there's no denying it inspired Starkey quite a bit.

## Starkey's Core Values
Starkey is a static & strongly typed programming language with a focus on data science, immutability, and safe alteration of data. As a language, Starkey follows a few major principles:

> 1. Data that is mutated or changed must be done so obviously.
> 2. Types assigned to data may never change and can only be temporarily type cast.
> 3. Control flow, including loops and recursive functions, should be easy to follow.
> 4. Anonymous functions should be well-implemented to allow for their streamlined use.
> 5. Operators and symbols should only be used for one purpose. Multiple actions should not share a single operator.
> 6. While whitespace and tabs are not tokenized, scope and context should be easy to follow and portray via the use of symbols and keywords.

## Key Features
There are several key features that move Starkey in the direction of these values.

### Immutability by Default
While on the surface this seems out of touch, it is designed to fit the core values of Starkey. One of the things that defines Starkey is it's deliberate approach to data manipulation. 

> Need to create a variable? Awesome. Let's do that.

> Need to mutate a variable? Well... Let's make sure you're sure what you're doing first.

Starkey variables require a `var` at definition if you want to change them later.
I chose `var` over `mut` because I think it's more in line with data science/mathematics to call it "variable" rather than computer science where "mutable" would be more familiar.

```none
new var i = 0
```

This prevents you from mutating a variable that you didn't mean to change, or causing unexpected behavior as the result of a function. Ultimately this is baby steps for the principles of Starkey. The real deal comes with `mut` keywords in function parameters and whatnot.

### The Mutate Operator
The mutate operator is another aspect of the immutability by default and deliberate manipulation of data. By using a different entire operator for setting versus modifying variables, we can effectively eliminate any scenario involving the accidental change of a variable.

```none
new var message = "Hello, World!"
$ message
message -> "Hello, Mom!"
$ message
```
Output:
```none
Hello, World!
Hello, Mom!
```

### Unique Operator Application
I have two very difficult goals to balance with Starkey:
1. I want the syntax to be recognizable and quickly learned
2. I want the syntax to be unique enough to where I have more freedom to implement built-in features for Starkey

One of the ways I try to accomplish this is to ensure that any operator only has one use case or application. This lead me to a crossroad...

One of the ways a language like lua does this is with keywords:
```lua=
local i: int = 10
if i < 5 then
    print("I is less than 5")
end
```
As you can see, it uses 'then' and 'end' to denote what would typically be brackets. This leaves things like `:` available for type assignment.

In Python these rules are broken:
```python=
i: int = 10
if i < 5:
    print("I is less than 5")
```
The `:` is used for both the type assignment and to indicate the start of the `if` block. This is simply unacceptable for Starkey, and on top of that, the lack of operator or keyword to denote the end of the block means that Python HAS to consider whitespace and escape characters which is something I refuse to implement in Starkey.

Now, I love lua, but the keywords are just kind of a mess. They're too long and too much. They take away from the actual parts of the code that matter, like the condition in the `if` statement. 

**My Solution**
My solution to this problem is to adopt a lua-like philosophy where there are keywords used, however, they're designed to be nice and short
```
new i: int = 10
if i < 5 do
    $ "I is less than 5"
end
```

The only other option would be to have `::` as the type assignment operator and `:` to denote the start of a block, which I think looks good, but variable declaration becomes really long: `new i :: int = 10`

### Starkey is Not General Purpose
One of the key things I'm trying to get at with Starkey between it's implementation and it's syntax is that it is not a general purpose language. Starkey isn't meant to build software. It's meant specifically to do data science, visualization, and manipulation.

This should be reflected in it's syntax and implementation. It shouldn't feel like a general purpose language. That's why I have operators like `$` which are used specifically for logging because this is something that you use a lot in these kinds of scenarios. The same is true of the `#` operator (which I totally stole from lua, by the way) because accessing the length of an array should be easier than having to type the whole name and call `.len()` on it.

### Logger Operator 
The Logger Operator is a simple but extremely powerful tool for Starkey. 

The default and most simple version `$` where anything after it will be printed to the console. 

```
proc entry():
    $ "Hello, world!"
end
```

This is all there is to the Starkey Hello World program. 

This operator becomes much more powerful when augmented. It can be augmented according to a number of rules.

| Operator(s) | Output |
| ----------- | ------- |
| `$addr` | Memory address of the value |
| `$type` | The type of the value |
| `$.2` | The value to 2 decimal points |
| `$center` | The value centered in the console |
| `$right` | The value right-aligned in the console |
| `$=` | Assert the following expression is true |
| `$!` | Error message |
and more to come

The logger operator can also use `{}` for printing multiple values at once. Any normal logger augments can then be placed inside these brackets

```
new value = 3.14

$ "{.2} and {.5}" value, value / 4.11323
```