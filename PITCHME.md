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
Product type
```csharp
public class Something {
  public int SomeNumber { get; set; }
  public string SomeSentence {get; set;}
}
```
@[2](All correct values of int are correct here)
@[3](All correct values of string are also correct here)
@[2-3](All combination of int and string are correct for the class 'something'. In short 'Something' is the product type of int * string )

+++

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
