---
title: F#
category: F#
layout: 2017/sheet
tags: [New]
updated: 2018-08-30
weight: -10
---
## Getting started
{: .-three-column}

### Hello world
{: .-prime}

#### Basic F# program
{: .-file}

```fsharp
open System

let greetMe name =
    sprintf "Hello, %s!" name

[<EntryPoint>]
let main argv =
    let message = greetMe "World"
    printfn "%s" message
    0
```

```
dotnet run
```

Download [.NET Core](https://www.microsoft.com/net/learn/fsharp/get-started-with-fsharp-tutorial) to run on your machine.

### Values
{: .-setup}

#### let bindings define immutable values

```fsharp
let x = 12
let name = "Don Syme"
```

#### let mutable defines a mutable value

```fsharp
let mutable x = 12
x <- 13
```

Bindings are immutable by default in F#. See [Values](https://docs.microsoft.com/dotnet/fsharp/language-reference/values/) for more.

### Functions
{: .-setup}

#### let bindings define functions

```fsharp
let square x = x * x

let isOdd x = x % 2 <> 0
```

#### Use |> to chain function calls together

```fsharp
let sumOfOddSquares items =
    items
    |> List.filter isOdd
    |> List.sumBy square
```

Functions are first-class in F#. See [Functions](https://docs.microsoft.com/dotnet/fsharp/language-reference/functions/) for more.

## Basic types
{: .-three-column}

### Strings

```fsharp
let s1 = "this is a string"
let s2 = @"this is a verbatim string"
let s3 =
    """
        this is a verbatim string
        that lets you use quotes ""!
    """
```

Strings all are of type `String` in .NET.

### Common Types

#### Basic numeric types

```fsharp
let x = 1      // int
let y = 1.0    // float
let z = 1.0m   // decimal
```

#### Other numeric types

```fsharp
let x = 1u       // unsigned int
let y = 1uy      // unsigned byte
let z = 2.3E+32  // other float notation
```

See [Literals](https://docs.microsoft.com/dotnet/fsharp/language-reference/literals) for more.

### Unit

```fsharp
// Function that takes nothing as does nothing
let f () = ()

let x = ()

// Ignore function produces nothing as output
12 |> ignore
```

Unit indicates the absence of a value. See [Unit Type](https://docs.microsoft.com/dotnet/fsharp/language-reference/unit-type) for more.

## Basic collections
{: .-three-column}

### Lists

#### Lists use `[ ]` syntax

```fsharp
let xs = [ 1; 2; 3; 4; 5 ]
let ys = [
    1
    2
]

let zs = xs |> List.map (fun x -> x + 1)
// [ 2; 3; 4; 5; 6 ]
```

#### List expressions and slices

```fsharp
let evensUnderTen =
    [ 1 .. 10 ]
    |> List.filter (fun x -> x % 2 = 0)

let tenSquares =
    [ for x in 1 .. 10 -> x * x ]

let nested = [ [1; 2]; [3; 4]; [5; 6] ]

let lst = [ for l in nested do
                for x in l ->  x * x ]
// [ 1; 4; 9; 16; 25; 36 ]
```

Lists are immutable, singly-linked lists.

### Arrays

#### Arrays use `[| |]` syntax

```fsharp
let xs = [| 1; 2; 3; 4; 5 |]
let ys = [|
    1
    2
|]

ys.[0] <- 5 // Change first item to be 5

let zs = xs |> Array.map (fun x -> x + 1)
// [| 2; 3; 4; 5; 6 |]
```

#### Array expressions and slices

```fsharp
let evensUnderTen =
    [| 1 .. 10 |]
    |> Array.filter (fun x -> x % 2 = 0)

let tenSquares =
    [| for x in 1 .. 10 -> x * x |]

let nested = [| [1; 2]; [3; 4]; [5; 6] |]

let lst = [| for l in nested do
                 for x in l ->  x * x |]
// [| 1; 4; 9; 16; 25; 36 |]
```

Arrays have mutable contents.

### Sequences

#### Seq uses `seq { }` syntax

```fsharp
let xs = seq { yield 1; yield 2 }
let ys = [|
    yield 1
    yield 2
|]

let zs = xs |> Seq.map (fun x -> x + 1)
// seq [2; 3]
```

#### Seq expressions

```fsharp
// Fibonacci sequence
let rec fibs = seq {
    yield 0
    yield 1
    yield!
        fibs
        |> Seq.map2 (+) (fibs |> Seq.skip 1)
}

let firstTen = Seq.take 10 fibs
// seq [0; 1; 1; 2; ...]
```

Sequences are lazily-computed, so they can represent infinite sets.

## Data types
{: .-three-column}

### Records

#### Record declaration

```fsharp
type CartItem =
    { ProductCode: string;
      Qty: int }
type ActiveCartData =
    { UnpaidItems: CartItem list }

type PaidCartData =
    { PaidItems: CartItem list
      Payment: decimal }
```

#### Record instantiation and usage

```fsharp
// Records are created with a similar syntax
let cartItem =
    { ProductCode = "MyProductCode"
      Qty = 2 }

// Copy and Update syntax
let cartItem2 = { cartItem with Qty = 3 }

// Use '.' syntax to access data
printfn "Quantity: %d" cartItem.Qty
```

See [Records](https://docs.microsoft.com/dotnet/fsharp/language-reference/records) for more.

### Discriminated Unions

#### Discriminated Union declaration

```fsharp
// Simple DU handling various cases
type Shape =
    | Circle of float
    | EquilateralTriangle of double
    | Square of double
    | Rectangle of double * double

// Recursively-defined DU with generics
type BST<'T> =
    | Empty
    | Node of 'T * BST<'T> * BST<'T>
```

#### Discriminated Union instantiation

```fsharp
let circle = Circle 2.0f
let rectangle = Rectangle(3.0f, 4.0f)

// Single-node binary search tree
let bst = Node("hello", Empty, Empty)
```

See [Discriminated Unions](https://docs.microsoft.com/dotnet/fsharp/language-reference/discriminated-unions) for more.

### Units of Measure

#### Units of Measure declaration

```fsharp
// Define a new UoM
[<Measure>]
type cm

// Define a UoM based on an existing one
[<Measure>]
type ml = cm^3
```

#### Units of measure usage

```fsharp
// Unit conversions
let convertg2kg (x: float<g>) =
    x / 1000.0<g/kg>

// Extracting a scalar value
let unwrap (x: int<m>) =
    x / 1<m>
```

See [Units of Measure](https://docs.microsoft.com/dotnet/fsharp/language-reference/units-of-measure) for more.

## Pattern Matching
{: .-three-column}

### match expressions

#### Match on discriminated unions

```fsharp
type Color = Red | Blue | Yellow

// val colorName : Color -> string
let colorName color =
    match color with
    | Red -> "red"
    | Blue -> "blue"
    | Yellow -> "yellow"
```

#### Match on other data

```fsharp
// You can match on any data type,
// but you may need a discard case at the end.
let getName name =
    match name with
    | "Phillip" -> printfn "Hello, Phillip!"
    | "Ali" -> printfn "Hello, Ali!"
    | _ -> printfn "Goodbye!"
```

### function keyword

#### Partially apply pattern matching

```fsharp
open System

// Same DU as before
type Shape =
    | Circle of float
    | EquilateralTriangle of double
    | Square of double
    | Rectangle of double * double

// Partially applied Shape argument:
//
// val calcArea : (Shape -> float)
let calcArea = function
    | Circle r -> 2.0 * Math.PI * r
    | EquilateralTriangle s ->
        s * s * sqrt(3.0) / 4.0
    | Square s -> s * s
    | Rectangle(l, w) -> l * w
```

### Destructuring data

#### Pull data out of types

```fsharp
type Data = String of string | Int of int

let (String s) = getData()
printfn "String length: %d" s.Length

// Tuple destructuring
let (x, y) = (1, 2)
```

See [Pattern Matching](https://docs.microsoft.comdotnet/fsharp/language-reference/pattern-matching) for more.

## Object programming
{: .-three-column}

### Classes

#### Class declaration with members and interface implementation

```fsharp
type CustomerName(first, middle, last) =
    member this.FirstName = first
    member this.MiddleName = middle
    member this.LastName = last

    member this.PrintName() =
        printfn "%s %s %s" first middle last

    interface System.IDisposable with
        member __.Dispose() =
            printfn "Disposing....!"
```

### Abstract classes

#### Declared with a default implementation

```fsharp
type MathService() =
    abstract Add: int -> int -> int
    abstract Mult: int -> int -> int

    default this.Add x y = x + y
    default this.Mult x y = x * y
```

#### Declared without a default implementation

```fsharp
[<AbstractClass>]
type MathService() =
    abstract Add: int -> int -> int
    abstract Mult: int -> int -> int
```

### Interfaces

#### Interface declaration

```fsharp
type IMathService =
    abstract Add: int -> int -> int
    abstract Mult: int -> int -> int
```

#### Object expressions to implement interfaces

```fsharp
let adhocMathSvc =
    { new IMathService with
          member __.Add x y = x + y
          member __.Mult x y = x * y }

let res1 = adhocMathSvc.Add 1 2
let res2 = adhocMathSvc.Mult 3 4
```

## More Resources

- [F# homepage](https://www.microsoft.com/net/learn/languages/fsharp) _(microsoft.com/net/learn/languages/fsharp)_
- [F# docs](hhttps://docs.microsoft.com/en-us/dotnet/fsharp/) _(docs.microsoft.com/fsharp)_
- [F# for fun and profit](https://fsharpforfunandprofit.com/) _(fsharpforfunandprofit.com)_
- [F# Wikibook](https://wikibooks.org/wiki/F_Sharp_Programming) _(wikibooks.org/wiki/F_Sharp_Programming)_
- [F# Software Foundation](https://fsharp.org/) _(fsharp.org)_