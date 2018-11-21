Style.jl
========

UPDATE: Please see the Julia Docs Style Guide: https://docs.julialang.org/en/v1/manual/style-guide/ <br> This repository hasn't been updated in many years. Where it differs, prefer the Julia Docs guide, above.
------------

This document lays out, in a very rough draft form, the style guidelines
for Julia programming that I've started to impose on myself. I'd ask that
anyone making contributions to my packages consider following these
guidelines as well.

These guidelines are designed to encourage writing highly readable code
that performs well. They make explicit what I personally think is good
style.

They are numbered to make them easier to reference in a discussion of
code, but the numbering is not yet finalized.

# Naming Files and Packages

(1) File names end in `.jl`, except for shell scripts which should not have any explicit file type extension.

(2) GitHub repo names end in `.jl`.

(3) Package names *do not* end in `.jl`.

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

(5) Never use tabs instead of space characters as whitespace in code.

(6) Never place more than 80 characters on a line.

(7) Always include a single space after a comma:

**Good style**

    x[1, 2]

**Bad style**

    x[1,2]

(8) Never insert a space before a comma:

**Good style**

    x[1, 2]

**Bad style**

    x[1 , 2]
    x[1 ,2]

(9) Always insert a single space before and after an operator, except for the `^` and `:` operators, which never have spaces around them:

**Good style**

    1 + 1
    1^2
    1:5

**Bad style**

    1+1
    1 ^ 2
    1 : 5

(10) Contrary to many other style guidelines, the spacing before-and-after rule applies to keyword arguments as well:

**Good style**

    foo(a = 1)

**Bad style**

    foo(a=1)

(11) Use explicit parentheses with the `:` operator in complex expressions. Do not rely upon Matlab-like precedence rules.

**Good style**

    1:(n - 1)

**Bad style**

    1:n - 1

# Naming Conventions

(12) When naming variables or functions, use short lowercase names if possible:

**Good style**

    isna

**Bad style**

    isNotAvailable, is_not_available

(13) If a variable or function name is too long to be read in all lowercase, insert underscores at word boundaries:

**Good style**

    lookup_table

**Bad style**

    lookupTable, LookupTable

(14) When naming mutable or immutable types, use initial-cap camelcase:

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

(15) When naming modules, including packages, use initial-cap camelcase, except for acronyms, for which all letters should be capitalized:

**Good style**

    module MyModule
        foo(x::Any) = 1
    end

    using MyPackage
    using GLM

**Bad style**

    module myModule
        foo(x::Any) = 1
    end

    module my_module
        foo(x::Any) = 1
    end

    using my_package
    using myPackage
    using Glm
    using glm

(16) When naming constants, use all caps:

**Good style**

    const MAGICNUMBER = 1

**Bad style**

    const magicnumber = 1
    const magic_number = 1
    const magicNumber = 1
    const MagicNumber = 1

# Mathematical Notation

(17) Always add explicit zeros to the ends of floating point constants:

**Good style**
    1.0 + 2.0

**Bad style**
    1. + 2.

# The Type System

(18) Always explicitly type all arguments to a function. Explicit typing makes code safer to use and clearer to an unfamiliar user:

**Good style**

    foo(x::Real, y::Real; z::Real = 1) = x + y + z

**Bad style**

    foo(x, y; z = 1) = x + y + z

(19) When the desired types for a function are too generic to be tightly typed in Julia, use an explicit `Any`. This makes it clear that you intended for your code to work with any type of input.

**Good style**

    screamcase(x::Any) = uppercase(string(x))

**Bad style**

    screamcase(x) = uppercase(string(x))

(20) Don't explicitly introduce a parametric type rule for a function unless it's needed to ensure correctness:

**Good style**

    foo(x::String) = print(x)
    foo(x::Vector) = print(x)
    foo{T <: Real}(x::Vector{T}) = sum(x)

**Bad style**

    foo{T <: String}(x::T) = print(x)
    foo{T <: Any}(x::Vector{T}) = print(x)

(21) Try to order method definitions from least specific to most specific type constraints.

**Good style**

    foo(x::Any) = print(x)
    foo(x::String) = print(uppercase(x))

**Bad style**

    foo(x::String) = print(uppercase(x))
    foo(x::Any) = print(x)

# Peformance

(22) Avoid creating temporary arrays, especially in loops.

(23) Ensure that functions return a single type for each type signature of inputs.

(24) Ensure that the type of any variable's binding does not change over the body of a function.

# Code Organization

(25) Most code should exist in a package, except for isolated scripts. Make ad hoc packages to organize your own work.

(26) When writing packages, obey the package organization rules by placing code in `src` and tests in `test`.

# Testing

(27) Always write a separate test file for every source file you write. Specifically, place the tests for `src/foo.jl` in `test/foo.jl`.

(28) The contents of `test/foo.jl` should be surrounded by a module to keep variables from leaking out:

**Good style**

    module TestFoo
        @assert foo
    end

**Bad style**

    @assert foo

(29) Test the functionality of `src/foo.jl` by writing at least one test for every type/function definition in `src/foo.jl`. Ensure systematic code coverage.

(30) Avoid explicit types for variables inside code unless there is potential for bugs that you need to catch.

**Good style**

    function foo()
        x = 1
        return x
    end

**Bad style**

    function foo()
        x::Int = 1
        return x
    end

# Comments

(31) Avoid overcommenting code. Focus on writing code that makes sense by using informative variable names and simple constructions. If you need to document a non-trivial algorithm or data structure, move that documentation into a specification file where it can be formatted nicely with diagrams and other information. English language documents are much more readable when they're not constrained by the rules for code comments.

(32) Write separate specification documentation for non-obvious algorithms.

# Be Conservative

Julia often gives you more freedom than you should use. Here are some guidelines for exhibiting self-control in the face of temptation.

(33) Don't use `importall`. Don't even use `import`. Explicitly annotate the source of each extended function at the point of extension:

**Good style**

    Base.mean(x::MyNewType) = 1.0

**Bad style**

    import Base.median
    median(x::MyNewType) = 1.0

**Worst style**

    importall Base
    median(x::MyNewType) = 1.0

(34) Always use `for x in y`. Never use `for x = y`.

(35) Always initialize `BigFloat` objects using a string literal, not a floating point literal. The floating point literal has low precision, which will be propagated to the `BigFloat`.
