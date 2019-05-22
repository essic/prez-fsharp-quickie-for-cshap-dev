### C`#` developers here's 6 reasons to have a serious look at F`#` 

---

> "Point of view is worth 80 IQ points" <br> Alan Kay

---

### What's F# ?

- Strongly, statically typed, open sourced and cross platform language with deep inference from Microsoft |
- Multi-paradigm language : functional, imperative and object-oriented |
- Heavily influenced by ML, OCaml, Haskell, C# & others |
- 1.0 appeared on 2005 ( older than GO, TypeScript, Rust ...) |

---

### Reason 1
#### No compatibility risk !

+++
C# + F# = LOVE

+++

! F# compilers features !

+++
In short, same constraints as VB.NET ...

---

### Reason 2
#### F# has better typing 

##### string * int
##### string + int

+++
#### Product type
```csharp
public class Something {
  public int SomeNumber { get; set; }
  public string SomeSentence {get; set;}
}
```
@[2](Any value of 'int' type can be assigned here)
@[3](Any value of 'string' type can be assigned here)
@[2-3](A valid instance of 'Something', can be any combination of 'int' and 'string')
@[2-3](=> 'Something' is a PRODUCT type of 'int' AND 'string' => int * string)

+++
#### Product type (again !) 

```fsharp
type Something = Ctr of int * string

type Something2 = { SomeNumber : Int ; SomeSentence : string }
```
@[1](Something is still product type, of 'int' AND string with 'Ctr' being the constructor for a value of type 'Something')
@[3](Something2 is a record YET - another kind of product type - )

+++
```fsharp
type Something() =
  member val SomeNumber = 0 with get, set
  member val SomeSentence = "" with get, set
```
Of course you can also make a class in F# ! Which are also PRODUCT types !

+++
#### Sum type

```fsharp
type Something =
    | Str of string
    | Nb of int
```
@[2](A value of type 'Something' can be a 'string')
@[3](A value of type 'Something' can also be a 'int')
@[1-3]('Something' is a SUM type of 'string' OR 'int' => int + string)


+++
NO Sum Type in C# but, there's polymorphism ...

+++

```csharp
public abstract class Credentials {
//...
}

public class ClassicCredentials : Credentials {
  public string User { get; set; }
  public string Password { get; set; }
}

public class TokenCredentials : Credentials {
  public string Token {get;set;}
}

public interface HasCredentials {
  Credentials GetCredentials();
} 

public class LoginSystem {
  public bool LogIn(HasCredentials subject) {
    bool hasSignedIn;
    //Imagine some code here that ...
    return hasSignedIn;
  }
}

public class UserWithLoginAndPassword : HasCredentials {
  //...
}

public class UserWithToken : HasCredentials {
  //...
}
```
@[1-3](We want a base Credentials class)
@[5-8](We define a type of Credentials)
@[10-12](We define another type of Credentials)
@[14-16](We define an interface to use later on ...)
@[18-24](We have some class which can deal with anything that implements 'HasCredentials')
@[26-28,30-32](We have two kind of Users, you get the idea ...)

+++
We got a lot of code for something we do almost everyday ... <br /> Let's see what we could do with F# 

+++

```fsharp
type UserAndPassword = { Login : string ; Password : string }

type Credentials =
| Token of string 
| Classic of UserAndPassword

type User =
  { 
    Cred : Credentials
    //other stuff there
  }
  
let logInToSystem(Credentials cred) =
  let hasSignedIn = false
  //imagination time ...
  hasSignedIn
```

@[1](Nothing new here)
@[3-5](This is a sum type.)
@[3-5](You create a valid value of type 'Credentials' with 'Token' OR 'Classic' constructor)
@[7-11](No need for several implementation of User or any variation, just give it a Credential field)
@[13-17](F# gives you ways to deal with both representation of Credentials)

+++
Sum type is not new : SML, OCaml, TypeScript, Rust, Scala, Haskell ...

---

### Reason 3
#### F# is immutable by default. 

##### I SHALL NOT WRITE READONLY, ANYMORE

+++
Far less complicated to write concurrent code in F#

+++
Declaration is initialisation

```fsharp
let someVariable : Int
```
@[1](This does not compile.)

+++
In F#, you never question if the value is present but only what the value is.

+++
No more implicit value by default

+++
So 'null' is not the default value reference type. 0 is not de default value for int ... 

+++
F# supports 'null' however ...
- no implicit 'null' assignation is done |
- pure F# types are not nullable (still usable in C#) | 

+++

Of course if mutability is needed ...

```fsharp
let mutable myNameIs = "@essicf37"

myNameIs <- "Slim Shady"
```
+++
Why the 'buzz' on immutability ? Please watch [@KevlinHenney presentation on NCraft](http://videos.ncrafts.io/video/276832516) for some answers.

---

### Reason 4
#### Functional paradigm first

#### `F#` supports several paradigm but is functional first 

+++
```fsharp
let sayHello name =
  printfn "Hello %s" name
```
This is a function in F# and it's strongly typed !

+++
```fsharp
//int -> int -> int
let add number1 number2 =
  number1 + number2

//int -> int
let addTwo = add 2

addTwo 4
//results is 6
```
@[1-3](A simple addition function)
@[3](No need for return, F# function always returns the last value computed)
@[5-6](We use the automatic currying of F# to create a function which will add '2')
@[8-9](We call the addTwo function)

+++
```fsharp
let add n1 n2 =
  n1 + n2

let mul n1 n2 =
  n1 * n2
  
let mulTwo = mul 2
let addTen = add 10

let mulByTwoThenAddTen = mulTwo >> addTen

mulByTwoThenAddTen 5 
//result is : 20

```
@[1-2,4-5](We define some functions)
@[7-8](We use some currying, again)
@[10](We use 'function composition' to define a function which multiply input by 2 then add 10 to the result)
@[10](This is called function composition, `>>` is offered by F#)
@[12-13](We call function mulByTwoThenAddTen)
+++

Also functional paradigm has 3 core operations (amongst others), all present in F# of course 
- Transform |
- Filter |
- Reduce |

+++
In short, it's about writing LinQ all day long :
- Transform : Select() in Linq or map in F# | 
- Filter: Where() in Linq or filter in F#|
- Reduce: Aggregate() in Linq or fold / reduce in F# |

+++
Functional paradigm implies a strong focus on data and composability of functions 

---

### Reason 5
#### Pattern matching

##### Just write it as it is ...

+++
```fsharp
type LoginAndPassword { Login: string; Password : string }
type Credentials =
| Token of string
| Classic of LoginAndPassword

let logInToSystem (subject:Credentials) =
  match subject with
    | Token t -> // We do something with it
    | Classic c -> // We do something else with it 
```
@[1-4](We take back our exemple from before ...)
@[6-9](We go a little further on the 'log in' function ...)
@[7-9](That's pattern matching !)
@[7,8,2,3](We deal with the Token representation of the sum type)
@[7,9,2,4](We deal with the Classic representation of the sum type)

+++
```fsharp
let someFunc a =
  let aBigTuple = ("@essiccf37","c# developer","f# deveoper")
  let handle, skill1, skill2 = aBigTuple
  //...
  ()
```
@[1-5](Some function)
@[2](A simple tuple, also a product type btw : `string * string * string`)
@[3](That's also pattern matching !)

+++
```fsharp
let someFunc (a: string list) =
  match a with
  | [] -> printfn "List is empty !" 
  | [_; _] -> printfn "List has only 2 elements"
  | _ -> printfn "List has more than 2 elements"
```
@[1-5](A function with pattern matching on list)
@[2-3](We do that if list is empty)
@[2,4](Or we do that if list has only two elements)
@[2,5](Or for any other case that's what we do !)

+++
Pattern matching in F# allows for much more, check out [Microsoft doc on this](https://docs.microsoft.com/en-us/dotnet/fsharp/language-reference/pattern-matching)

---
### Reason 6
#### Interactivity is key !

+++
F# has a REPL call FSI for F# Interactive

+++
FSI has full Visual Studio Support for a while now, VS2017 included.

+++
REPL is useful: ask Python and Javascript developers or just watch the CScript initiative from MS ...

+++
Some useful usage :
- You can create scripts |
- You can write code to test some cases and load production DLLs to see exactly what's happening  |
- You can run some code during developmenet, load it and try it directly |

---
### Demo !

Showing information about collaborators <br> @fa[arrow-down]

+++
### 2 kinds
- Consultants |
- Managers |

+++
### Basic information
- First name |
- Last name |
- Age |

+++
### Also ...
- Managers handles business unit |
- Consultants have skills |

---

### First implementation
@fa[arrow-down]

+++?code=src/implem01.fs&lang=fsharp
@[1](We create our module)
@[3](Basic import, useful to access String type)
@[6-9](We define a basic record, representing a person. This is one kind of product type)
@[12-22](These represents possible skills and business unit values. These are sum types)
@[25-31](This represent a collaborator. Many things here.)
@[34-37](Function use to create a Manager value)
@[35](We create a Person)
@[36](We create a Tuple - Product Type - of Person and ManagerBU)
@[37](We create and return a Manager value, which is of type Collaborator)
@[40-43](Function use to create a Consultant value. Same thing !)
@[46-50](Pretty print age, first name and last name to the console. Also meet 'Pattern Matching' !)
@[47](This is it !)
@[48](Match this pattern, do this !)
@[49](Match that pattern, do that !)
@[52-67](Pretty print what a collaborator does. Hello again Pattern matching ! Also welcome to map & reduce !)
@[53-54](We handle Manager first)
@[53,55](Now we handle consultant)
@[55-57](Let's match on empty list of skills first for consultant)
@[55,56,58-66](For any other case ...)
@[59-62](Map this !)
@[65](Reduce that !)
@[69-74](Pretty print collaborator information with function composition)
@[71-72](Yes, right here)
@[75-87](Main function !)

+++
[REPL here](https://repl.it/@essic/whyFsharpDemo1-public)

---

### Inference magic
@fa[arrow-down]

+++?code=src/implem02.fs&lang=fsharp
@[34-37,40-43](We remove types, F# is smart enough to figure out things)
@[46-50]
@[52-67]
@[69-74]
@[77-87]

+++
[REPL here](https://repl.it/@essic/whyFsharpDemo2-public)

---

### Naive error handling in F# !
@fa[arrow-down]

+++
We wish to forbid the creation of a person with an invalid name and / or first name

+++
Let's say an invalid name / first name is null or empty here

+++
For an invalid age, since we're cool individuals, we'll just forbid negative number

+++?code=src/implem03.fs&lang=fsharp
@[34-37](We create a function which check if I got any nullable string is my list !)
@[37](We use a Folding to do that ! Reduce is in fact a kind of Fold :p)
@[39-49](We create a function to create a person or fail if any entry is invalid, let's use the 'T option' type ...)
@[39-41](This is our guy ! A sum type, yes.)
@[45-46](If we got an invalid entry we return 'None' which is a valid value of the 'Person option' type !)
@[45,48-49](If all is right, we return 'Some person' which is also a valid value of the type 'Person option' type !)
@[52-59](Let's use it to create a manager or fail if entries are invalid)
@[54]
@[56-57](Since we are not sure of the 'person', we use Option.map)
@[56-58](Same here)
@[59](So we return a value of the 'Collaborator option' type)
@[62-67](Same here)
@[70-74](We change nothing here)
@[76-98](Here as well ...)
@[100-118](Our main did change a little...)
@[108-110](Let's add some bad entries !)
@[112-117](The magic is here !)

+++
[REPL here](https://repl.it/@essic/whyFsharpDemo3-public) <br> @fa[arrow-down]

+++
### There are better ways ! 
@fa[arrow-down]

+++
Make illegal state non representable
```fsharp
type FirstName = private T of string

module FirstName =
  // String -> Option<FirstName>
  let create str =
    if String.IsNullOrWhiteSpace(str) then
      None
    else
      T str |> Some
```

+++
Using Result and its operators
```fsharp
type FirstName = private T of string

module FirstName =
  // type Result<TSuccess,TError> =
  // | Ok of TSuccess
  // | Error of TError
  
  // String -> Result<FirstName,String>
  let create str =
    if String.IsNullOrWhiteSpace(str) then
      Error "Can't initialize firstname with empty or null value !"
    else
      T str |> Ok
let doSomething () =
  //...
  FirstName.create someVariable 
  |> Result.map ( doSomethingWithFirstIfSuccess )
  |> Result.mapError ( handleErrorIfFailure )
```

+++
And a lot more to explore ...

---
### One small currying example 
@fa[arrow-down]

+++?code=src/implem04.fs&lang=fsharp
@[101-125]
@[103-104](An alternative definition to create a consultant)
@[107](Currying !)
@[109-117](We now use it)

+++
[REPL here](https://repl.it/@essic/whyFsharpDemo4-public)



---
### What we did not talk about 

+++ 
Support on multiple IDE <br /> (VS, Atom, VsCode, Emacs, Vim) 

+++ 
F# on .NET core

+++ 
Active patterns 

+++ 
Error handling with : `Option<T>` and `Result<TSuccess,TError>` 

+++ 
Type Providers

+++ 
Asynchronous programming

+++ 
Computation expression

+++ 
[Domain Driven Design with F#](https://www.amazon.fr/Domain-Modeling-Made-Functional-Domain-Driven/dp/1680502549/ref=sr_1_fkmr0_1?ie=UTF8&qid=1531145812&sr=8-1-fkmr0&keywords=DDD+in+F%23)

+++ 
Property based testing with [FSCheck](https://fscheck.github.io/FsCheck/) or [Hedgehog](https://github.com/hedgehogqa/fsharp-hedgehog)

+++ 
F# to Javascript with [Fable.io](http://fable.io)

+++ 
Much more features, libraries and frameworks

---
### Some useful links

- Learning F# for [Fun and Profit](https://fsharpforfunandprofit.com)
- [Interactive programming with F#](https://docs.microsoft.com/en-us/dotnet/fsharp/tutorials/fsharp-interactive/)
- [Weekly news on F#](https://sergeytihon.com/category/f-weekly/)
- [F# foundation](https://fsharp.org)

---

C# is absolutely not dead and continues to evolve <br /> However having another point of view in itself can only make you better at what you do. 

---
### Thank you !

Questions ?
