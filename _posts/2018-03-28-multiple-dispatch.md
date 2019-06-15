---
layout: post
title: Multiple dispatch and hierarchical data types in Julia
tags: Julia
snippet: Julia is a programming language designed for numerical computing, and it supports multiple dispatch and type hierarchies.
author: Cory Simon
---

In our research group, we write our codes to conduct molecular simulations in the Julia programming language. We choose Julia because it is a high-level, dynamic language, but, owing to its design and just-in-time compiler, Julia approaches the speed of C. Compared to writing code in C, Julia enables us to write similarly fast code with fewer lines and lower complexity.

In this post, we give examples to show two powerful features of Julia: hierarchical data types and multiple dispatch.
Let's use wine as an example.

# Hierarchical data types

In Julia, we can define an abstract type, `GlassOfWine`, for a glass of wine:

```julia
abstract type GlassOfWine end
```

Then we can define two subtypes of glasses of wine, one for Pinot Noir and one for Grenache. Let's say all we care about is the region the wine is from and the volume we have in our glass. But, for Pinot Noir, we're also concerned about the price.

```julia
type PinotNoir<:GlassOfWine
    region::String
    volume::Float64 # ounces
    price::Float64 # $
end

type Grenache<:GlassOfWine
    region::String 
    volume::Float64 # ounces
end
```

The `<:GlassOfWine` part declares that `PinotNoir` and `Grenache` are subtypes of `GlassOfwine`. You can confirm by `subtypes(GlassOfWine)` in Julia. Or `supertype(PinotNoir)` and `supertype(Grenache)`, which will return `GlassOfWine`.

With this abstract type `GlassOfWine`, we can write *one* function, `sip!`, that operates on *both* types of wine glasses.

```julia
function sip!(glass_of_wine::GlassOfWine)
    glass_of_wine.volume -= 0.1
end
```

The function `sip!` (the exclamation signifies that we are modifying the argument) operates on glasses of both Pinot Noir and Grenache.

```julia
wg = PinotNoir("Oregon", 5.0, 25.0)
sip!(wg)
wg.volume # 4.9

wg = Grenache("Spain", 5.0)
sip!(wg)
wg.volume # 4.9
```

That's a simple example of a type hierarchy in Julia.

# Multiple dispatch

In Julia, we can write a function with the same name that behaves differently depending on the data type passed to it. For example, `guess_region` will guess the region a `GlassOfWine` is from, but will guess a different region depending on whether it is a `PinotNoir` or a `Grenache`.

```julia
function guess_region(glass_of_wine::PinotNoir)
    println("Is this glass of wine from Oregon?")
    glass_of_wine.region == "Oregon" ? println("Yes") : println("No")
end

function guess_region(glass_of_wine::Grenache)
    println("Is this glass of wine from Spain?")
    glass_of_wine.region == "Spain" ? println("Yes") : println("No")
end
```
Now if we have two glasses of wine, each from Oregon, but one Pinot Noir and one Grenache, `guess_region` will behave differently for each glass:

```julia
pn = PinotNoir("Oregon", 5.0, 25.0)
g = Grenache("Oregon", 5.0)

guess_region(pn)
# Is this glass of wine from Oregon?
# Yes
guess_region(g)
# Is this glass of wine from Spain?
# No
```
