---
layout: post
title:  "New Features in C# 7"
date:   2018-01-05 14:10:51 +0800
categories: .net
tags: .net
description: new released feature.
---
Here you can find a summary of the most important features that were released with c# 7.

This summary was made mainly for myself as a go to place in case I want to remember what I read.

The resources I recommend for having each item described in detail are the following:

   1. [Pluralsight course on  C# 7: First Look][Pluralsight-Csharp-7] by mister [Jesse Liberty][Jesse-Liberty] 

   2. [MSDN][MSDN]


## Static Using

Before:

  ![w]({{ site.baseurl }}//assets/images/new-features-c-7/before-static-using.png)


Now:

  ![w]({{ site.baseurl }}//assets/images/new-features-c-7/static-using1.png)
  ![w]({{ site.baseurl }}//assets/images/new-features-c-7/static-using2.png)

## Tuples

The code written with tuples in previous .net frameworks can become very verbose and difficult to comprehend.  Therefore a few improvements were done in c# 7 to overcome this.


Examples with tuples used as return values as literals and with named values:

    ![]({{ site.baseurl }}//assets/images/new-features-c-7/tuples1.png)


Use tuples in dictionaries as keys

    ![]({{ site.baseurl }}//assets/images/new-features-c-7/tuples2.png)

Deconstructing tuples

Tuples can be deconstructed during declarations by using the corresponding type or by using var, or by using local variables.

  Example

    ![]({{ site.baseurl }}//assets/images/new-features-c-7/tuples3.png)

## Pattern Matching

They are brought as enhancements of two existing constructs:

Is expressions can have a patterns on the right hand side, not just types
Case clauses in switch statements can now match on patterns and conditionals, not just constants
Pattern Matching in Is expressions:

    ![]({{ site.baseurl }}//assets/images/new-features-c-7/pattern3.png)

Pattern Matching in Case clauses:

    ![]({{ site.baseurl }}//assets/images/new-features-c-7/pattern4.png)

 

## Local Functions

Functions declared within another function are now supported.

The main usage for this feature would be to have a helper method inside the function they help.

I just hope no one will get to use this. The legacy code that has to be maintain and understood nowadays contains enough complex, big methods.

I would recommend to stick with the clean code principles, and try to keep the code base as simple as possible.


## Out Variables Literals

Before:

Out variables had to be declared before calling the method, but without using the var to declare them.

  Example:

    ![]({{ site.baseurl }}//assets/images/new-features-c-7/outer1.png)


Now:

The variables can be declared inside the calling method.

  Example:

    ![]({{ site.baseurl }}//assets/images/new-features-c-7/outer2.png)



## Ref. Returns

The value types can be converted into reference values by using the “ref” keyword.


## Use Exceptions as Expressions

    ![]({{ site.baseurl }}//assets/images/new-features-c-7/exceptions-as-arguments.png.png)

 

[Pluralsight-Csharp-7]: https://www.pluralsight.com/courses/csharp-7-first-look 
[Jesse-Liberty]: https://app.pluralsight.com/profile/author/jesse-liberty 
[MSDN]: https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-7
