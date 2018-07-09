### C# developers: 6 Reasons to have a serious look at F#

---

> "Point of view is worth 80 IQ points" <br> Alan Kay

---

### What's F# ?

- Strongly, statically typed, open sourced and cross platform language from Microsoft with deep inference |
- 1.0 appeared on 2005 ( older than GO ...) |
- Multi-paradigm language : functional, imperative and object-oriented |

---

### What's F# ?

- It compiles to executable or you can script with it |
- Heavily influenced by ML, OCaml, Haskell, C# & others |
- Fully supported by .NET & Mono. Compiles to Javascript |

---

### Reson 1
#### No compatibility risk !

+++
Anything written in C# can be used in F# !

+++
In fact it's true for everything running on the .NET Runtime... 

+++

The reverse is true for all basic F# that you can write. <br /> Special cases includes features depending on the F# compilers.

---

### Reason 2
#### F# has better typing 

Sum types & Product types 

+++
#### Product type
```csharp
public class Something {
  public int SomeNumber { get; set; }
  public string SomeSentence {get; set;}
}
```
@[2](All correct values of int are correct here)
@[3](All correct values of string are also correct here)
@[2-3](All combination of int and string are correct for the class 'Something'.<br /> In short 'Something' is a product type of int and string )

+++
Product type in F#

```fsharp
type Something = Ctr of int * string

type Something2 = { SomeNumber : Int ; SomeSentence : string }
```

@[1](Something is a product type, of int and string with 'Ctr' being the constructor)
@[3](Something2 is a record - another kind of product type - )

+++
```fsharp
type Something() =
  member val SomeNumber = 0 with get, set
  member val SomeSentence = "" with get, set
```
Of course you can also make a class in F# !

+++
#### Sum type

It's when a type can have multiple representation. <br /> There's no Sum Type in C# however, there's polymorphism ...

+++

```csharp
public abstract class Credentials {
//...
}

public class LoginAndPasswordCred : Credentials {
  public string User { get; set; }
  public string Password { get; set; }
}

public class TokenCred : Credentials {
  public string Token {get;set;}
}

public interface HasCredentials {
  Credentials GetCredentials();
} 

public class LoginSystem {
  public bool LogIn(HasCredentials subject) {
    bool hasSignedIn;
    //Imagine some code here ...
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
@[1-3](We want a Credentials class)
@[5-8](We define a type of Credentials)
@[10-12](We define another type of Credentials)
@[14-16](We define an interface to force the existence of Credentials later on ...)
@[18-24](We have some class which can deal with anything that implements 'HasCredentials')
@[26-28,30-32](We have two kind of Users, you get the idea ...)

+++
Lot's of code for something we do almost everyday ... <br /> Let's check what we could do with F# ...

+++

```fsharp
type UserAndPassword = { Login : string ; Password : string }

type Credentials =
| Token of string 
| Classic of LoginAndPassword 

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
@[3-5](You create a valid Credentials value with the Token or the Classic constructor)
@[7-11](No need for several implementation of User or any variation, just give it a Credential field)
@[13-17](F# gives you ways to deal with both representation of Credentials)

---

### Reason 3
#### F# is immutable by default. 

How many times have you declared some fields / properties as 'readonly' ?

+++
Far less complicated to write concurrent code in application in F#

+++
Declaration is initialisation.

```fsharp
let someVariable : Int
```
This does not compile in F#, you must give it a value

+++
You never question if the value is present when needed except if the type tells you it can happen

+++
'null' is not the default for representing the abscence of value anymore ...

+++
#### F# supports 'null' for compatibility reasons with .NET, however ...
- no implicit 'null' assignation is done
- pure F# types are not nullable 

+++

Of course if mutability is what you want ...

```fsharp
let mutable myNameIs = "@essicf37"

myNameIs <- "Slim Shady"
```
+++
Why immutability ? Please watch [@KevlinHenney presentation on NCraft](http://videos.ncrafts.io/video/276832516)

---

### Reason 4
#### Functional paradigm first

As stated before, F# supports several paradigm but is functional first. 

+++
```fsharp
let sayHello name =
  printfn "Hello %s" name
```
This is a function in F# and it's strongly type !

+++
```fsharp
//int -> int -> int
let add number1 number2 =
  number1 + number2

//int -> int
let add2 = add 2

add2 4
//results is 6
```
@[1-3](A simple addition functions)
@[3](No need for return, F# function always returns the last value computed)
@[5-6](We use the automatic currying of F# to create a function which will add '2')
@[8-9](We call the add2 function)

+++
```fsharp
let add n1 n2 =
  n1 + n2

let mul n1 n2 =
  n1 * n2
  
let mul2 = mul 2
let add10 = add10

let mulBy2ThenAdd10 = mul2 >> add10

mulBy2ThenAdd10 5 
//result is : 20

```
@[1-2,4-5](We define some function)
@[7-8](We use some currying)
@[10](We use 'function composition' to define a function which multiply input by 2 then add 10 to the result)
@[10](This is called function composition, '>>' is offered by F#)
@[12-13](We call that function)
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
Functional paradigms implies a strong focus on data, function working on data and composability. 

---

### Reason 5
#### Pattern matching

Just write it as it is ...

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
@[6-9](We go a little further on the log in function ...)
@[7-9](That's pattern matching !)

+++
```fsharp
let someFunc a =
  let aBigTuple = ("@essiccf37","c# developer","f# deveoper")
  let handle, skill1, skill2 = aBigTuple
  //...
  ()
```
@[1-5](Some function)
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
@[2,5](For any other case that's what we do !)

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
REPL is useful ask Python and Javascript developers or just watch the CScript initiative from MS ...

+++
Some useful usage :
- You can create scripts |
- You can write code to test some cases and load production DLLs to see what's happening exactly |
- You can run some code during developmenet, load it and try it directly |

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
Asynchronuous programming

+++ 
Computation expression

+++ 
[Domain Driven Design with F#](https://www.amazon.fr/Domain-Modeling-Made-Functional-Domain-Driven/dp/1680502549/ref=sr_1_fkmr0_1?ie=UTF8&qid=1531145812&sr=8-1-fkmr0&keywords=DDD+in+F%23)

+++ 
Property based testing with [FSCheck](https://fscheck.github.io/FsCheck/) or [Hedgehob](https://github.com/hedgehogqa/fsharp-hedgehog)

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
### Thank you !

Questions ?
