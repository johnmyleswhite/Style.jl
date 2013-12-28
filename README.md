Style.jl
========

This document lays out, in a very rough draft, the style guidelines for Julia programming that I've started to impose on myself. I'd ask that anyone making contributions to my packages consider following these guidelines as well.

# Naming Files and Packages

(1) File names end in `.jl`

(2) GitHub repo names end in `.jl`

(3) Package names *do not* end in `.jl`

# Whitespace and Line Breaks

(4) Use four spaces when indenting:

**Good style**

    function foo(n::Integer)
        x = 0
        for i in 1:n
            x += i
        end
        return x
    end

**Bad style**

    function foo(n::Integer)
      x = 0
      for i in 1:n
        x += i
      end
      return x
    end

(5) Never use tabs instead of space characters

(6) Always include a single space after a comma:

**Good style**

    x[1, 2]

**Bad style**

    x[1,2]

(6) Never use a space before a comma:

**Good style**

    x[1, 2]

**Bad style**

    x[1 , 2]

(7) Always place a single space before and after an operator:

**Good style**

    1 + 1

**Bad style**

    1+1

(8) Use explicit parentheses with the `:` operator in complex expressions. Do not rely upon Matlab-like precedence rules.

**Good style**

    1:(n - 1)

**Bad style**

    1:n - 1

(9) Never place more than 80 characters on a line

# Naming Conventions

(9) For variables and functions, use short lowercase names whenever possible:

**Good style**

    isna

**Bad style**

    isNotAvailable, is_not_available

(10) If a variable or function name is long, use underscores:

**Good style**

    lookup_table

**Bad style**

    lookupTable, LookupTable

(11) Both mutable and immutable types use initial-cap camelcase:

**Good style**

    type Pair
        val1::Float64
        val2::Float64
    end

    immutable ImmutablePair
        val1::Float64
        val2::Float64
    end

**Bad style**

    type pair
        val1::Float64
        val2::Float64
    end

    immutable immutablePair
        val1::Float64
        val2::Float64
    end

    immutable immutable_pair
        val1::Float64
        val2::Float64
    end

(12) Modules use initial-cap camelcase:

**Good style**

    module MyModule
        foo(x::Any) = 1
    end

**Bad style**

    module myModule
        foo(x::Any) = 1
    end

    module my_module
        foo(x::Any) = 1
    end

(13) Constants use all caps:

**Good style**

    const MAGICNUMBER = 1

**Bad style**

    const magicnumber = 1
    const magic_number = 1
    const magicNumber = 1
    const MagicNumber = 1

# Mathematical Operations

(14) All binary operators, except `^`, must have a space before and after the operator:

**Good style**

    x + y
    1.0 + 3.4
    3^4

**Bad style**

    x+y
    1.0+3.4
    3 ^ 4

(15) Contrary to many other style guidelines, this rule applies to keyword arguments as well:

**Good style**

    foo(a = 1)

**Bad style**

    foo(a=1)

(16) Always add explicit zeros to the ends of floating point constants:

**Good style**
    1.0 + 2.0

**Bad style**
    1. + 2.

# The Type System

(17) Always explicitly type all arguments to a function. Explicit typing makes code safer and clearer:

**Good style**

    foo(x::Real, y::Real; z::Real = 1) = x + y + z

**Bad style**

    foo(x, y; z = 1) = x + y + z

(18) When the desired types for a function are totally generic, use an explicit `Any`. Make it clear that you intended for your code to work with any type of input.

(19) Don't explicitly parameterize types unless it's necessary to ensure correctness:

**Good style**

    foo(x::Vector) = print(x)
    foo{T <: Real}(x::Vector{T}) = sum(x)

**Bad style**

    foo{T <: Any}(x::Vector{T}) = print(x)

(20) Order method definitions from most specific to least specific type constraints.

# Peformance

(21) Avoid creating temporary arrays, especially in loops

(22) Ensure that functions return a single type for each type signature of inputs

# Code Organization

(23) Most code should exist in a package, except for isolated scripts

(24) Obey the package organization rules

# Testing

(25) Always write a separate test file for every source file you write.

(26) Place tests for `src/foo.jl` in `test/foo.jl`.

(27) The contents of `test/foo.jl` should be surrounded by a module:

**Good style**

    module TestFoo
        # MAGIC HAPPENS HERE
    end

(28) Test the functionality of `src/foo.jl` by writing at least one test for every type/function definition in `src/foo.jl`. Ensure systematic code coverage.

(29) Avoid explicit types for variables inside code unless there is potential for bugs that you need to catch.

# Comments

(30) Minimize comments. Write code that makes sense.

(31) Write separate specification documentation for non-obvious algorithms.
