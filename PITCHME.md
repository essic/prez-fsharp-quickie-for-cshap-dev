### 6 Reasons to have a serious look at F# for a C# developer.

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
No compatibility risk !

+++
Anything written in C# can be used in F# !

+++
In fact it's true for everything running on the .NET Runtime... 

+++

The reverse is true for all basic F# that you can write. <br /> Special cases includes features depending on the F# compilers.

---

### Reason 2

F# has better typing, which reduces boiler plate code drastically <br /> Sum types & Product types 

---
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

---
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
@[13-17](F# gives you way to deal with both representation of Credentials)

---

### Reason 3
F# is immutable by default. <br /> Immutability in general have deep repercusion on the design / implementation of systems. <br> Here some benefits for F# ...

+++
Declaration is initialisation.

```fsharp
let someVariable : Int
```
This does not compile in F#, you must give it a value

+++
Far less complicated to write concurrent code in application in F#

+++
You never question if the value is present when needed except if the type tells you, it can happen <br /> 'null' is not the default for representing the abscence of value ...

+++
#### F# supports 'null' for compatibility reasons with .NET, however ...
- no implicit 'null' assignation is done
- pure F# types are value type, not reference (a little bit more complicated ^__^)

+++

If you want to know more about immutability in general and why so many systems / practices uses it (Redux, Kafka, Event Sourcing ...) <br /> Please watch [@KevlinHenney presentation on NCraft](http://videos.ncrafts.io/video/276832516)

---

