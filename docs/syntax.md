# Declaring variables
Templates:
```
[1] 'new {identifier}: {type} = {value}'
[2] 'new {identifer} = {value}'
[3] 'new var {identifier}: {type} = {value}'
[4] 'new var {identifer} = {value}'
[5] 'new const {identifer}: {type} = {value}'
```

```
new x: int = 10                     // integer 10
new var i = 0                       // integer 0
new message: str = "Hello, World!"  // string "Hello, World!"
new const version: str = "1.1.0"    // string "1.1.0"
```

# Reassigning Variables
Templates:
```
[1] '{identifier} -> {value}'
[2] 'increment {identifier}'
[2] 'decrement {identifier}'
[3] '{identifier} += {value}'
[4] '{identifier} -= {value}'
[5] '{identifier} *= {value}'
[6] '{identifier} /= {value}'
```

```
new var i = 0

i -> 1
increment i
decrement i
i += 1
i -= 1
i *= 2
i /= 2
```

# Procedure Declaration
Templates:
```
[1] 'proc {identifier}({parameters}): {value}'
[2] 'proc {identifier}({parameters})'
``` 

```
proc add_five(x: int): int
proc print_result(result: str)
```

# Type Annotation
Templates:
```
[1] '{identifier}: {value}'
[2] '{identifier}: T where T is {types}'
```

```
proc add_five(x: int): int
proc print_result(T where T :: str | int | float)
```

# Type Equality
Templates:
```
[1] '{identifier} :: {type}'
[2] '{identifier} !: {type}'
```

```
new pi: float = 3.14159
new e: float = 2.71828
new n: int = 10

$= pi :: e  // true
$= pi !: e  // false
$= pi :: n  // false
$= pi !: n  // true
```

## Example of type equality being used
```
new users = ...
new data: {str | int} = {
    "Name" = "",
    "Age" = 0,
}

for _, user in users
    new var user_data = data.copy()
    
    for key, value in user.Info
        if value :: user_data[key]
            user_data[key] = value
        else
            $! "Attempted to assign a value of non-matching types to key {}" key
        end
    end
end
```

## Why the specifics for semantically variable parameters?
Semantically variable parameters are parameters that can have multiple types.
```
T: str | int | float | bool
```
On paper, it is seasier to simply put...
```
t: str | int | float | bool
```
However, I specifically chose to make everyone's life harder because when using these parameters, not accounting for differing functionality among types can be a major way to shoot yourself in the foot.

By using a capital letter for these variables (much like generics in other languages) you are reminded that you need to account for different types.

This seems overkill but it is an actual thing I experience when programming in lua.

The compiler is planned to do checks for this as well...

```
proc example(T: str | int)
    $ #T        // (prints the length of a string or array)
end
```

The compiler should error here because this code is only valid while T is a string. If T is an integer, the program will crash.

```
proc example(T: str | int)
    if T :: str 
        $ "T is a string"
        $ #T
    elif T :: int
         $ "T is an int"
        $ T
    end
end
```

```
proc example(T: str | int)
    match typeof(T)
        str = 
            $ "T is a string"
            $ #T
        end

        int =
            $ "T is an int"
            $ T
        end
    end
end
```
(this match syntax is WIP)

These are both valid solutions to this problem and will make the compiler stop putting our warnings.
