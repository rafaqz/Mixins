# Mixers

<img src="https://www.giraffescanbake.com/wp-content/uploads/2016/12/Pomegranate-Martini3.gif" data-canonical-src="https://www.giraffescanbake.com/wp-content/uploads/2016/12/Pomegranate-Martini3.gif" width="300" height="420" />

[![Build Status](https://travis-ci.org/rafaqz/Mixers.jl.svg?branch=master)](https://travis-ci.org/rafaqz/Mixers.jl)
[![Coverage Status](https://coveralls.io/repos/rafaqz/Mixers.jl/badge.svg?branch=master&service=github)](https://coveralls.io/github/rafaqz/Mixers.jl?branch=master)

Mixer provides mixin macros, for writing, well, "DRY" code.

The @mix and @premix macros generate custom macros that can add fields to any
composite struct, preserving parametric types. They can be chained together, and
even play well with @with_kw from Parameters.jl. 

@premix inserts fields and types at the start of the definition:

```juliarepl
@premix struct Fruitjuice{P,B}
   pommegranite::P
   orange::B
end

@Fruitjuice struct Punch{L}
    vodka::L
end

julia> fieldnames(Punch)

3-element Array{Symbol,1}:
 :pommegranite
 :orange      
 :vodka       

julia> punch = Punch(20, 150, 2.5)
               
Punch{Int64,Int64,Float64}(20, 15, 12.5) 
```

@mix puts them at the end:

```juliarepl
@mix struct Soda{M,B}
    soda::J
end

@Soda struct Drink{J}
    lemon::M
    lime::B
end

julia> fieldnames(Drinks)

3-element Array{Symbol,1}:
 :lemon
 :lime     
 :soda    

julia> d = Drink(1.9, 13, 7)
                  
Drink{Float64,Int64,Int64}(1.9, 13, 7)                
```


One gotcha is the need to put empty curly braces on a struct with no
parametric fields, if it is going to have parametric fields after @mix or
@premix.

```julia
@Fruitjuice struct Juice{} end
```

@pour generates simple macros that insert lines of code:

```julia
@pour milk begin
    "Yum"
end

taste() = @milk

julia> taste()                                                                      
"Yum"
```
