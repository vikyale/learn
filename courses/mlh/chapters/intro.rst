Intro to SPARK Ada
==================

:code-config:`run_button=True;prove_button=False`

.. role:: ada(code)
   :language: ada

Let’s take a quick tour of SPARK Ada.
-------------------------------------

We’re going to cover the language at a high level:
* If you’re familiar with C or C++, Ada will look very familiar.
* Our goal is to enable you to write your first Ada program!
* We’ll also cover some useful SPARK specific features.

.. todo:: insert lady ada picture

**Fun Fact:** Ada was named after Ada Lovelace, the first programmer!

Hello World.
------------

The first program we usually write in a new language is “Hello World”, which just prints those words.

.. code:: ada run_button

    with Ada.Text_IO; use Ada.Text_IO;
    procedure Main is
    begin
      Put_Line ("Hello, world!");
    end Main;

**So, what is this code doing?**
* **Line 1:** Import the Ada Text_IO package and use it.
* **Line 2 - 3:** Define a new procedure called “Main”.
* **Line 4:** Print the string “Hello, world!” to the console.
* **Line 5:** Close the procedure.

Data Types
----------

Your programs will need to interact with data. What types does Ada have to offer?

**Built in Types:**
Ada has a number of built in data types that you can use in your programs.  Today we’ll be using:
* Integer
* Character
* String
* Boolean

**Custom Types:**
You can also create your own types that build on the built in types.

.. code-block:: ada

    type My_Integer is range 1 .. 10;

    type RGB is (Red, Green, Blue);

Variables & Assignment.
-----------------------

You’re going to need some place to store data in your programs.  Variables hold data & allow you to access it.

**Assignment**
Assignment statements are written as :ada:`name := value`.  For example, :ada:`X := 10;` sets the variable :ada:`X` to :ada:`10`.

**Initialization**
Variable initializations use the following format:
:ada:`[NAME] : [TYPE] := [VALUE](CONSTRAINTS);`
For example:  :ada:`A : Integer := 10;`

**Naming**
In Ada, variable names are case insensitive. :ada:`HELLO` is the same as :ada:`Hello` is the same as :ada:`hello`.

**Note: = vs. :=**
In Ada, the :ada:`=` (single equal) is for comparison and :ada:`:=` is for assignment.

Strong Types.
-------------

Ada is a strongly typed language which means that variable types are predefined and do not change.

**Declaring Variables.**
Since Ada is strongly typed, we need to declare variables up front.

.. code-block:: ada

    declare
      X : Integer := 10;
    begin
      Do_Something (X);
    end;


**Mixing Types.**
Since Ada is strongly typed, you can’t combine objects of different types.

.. code-block:: ada

    Len_1 : Miles := 5.0;
    Len_2 : Kilometers := 10.0;

    -- This would cause an error!
    D : Kilometers := Len_1 + Len_2;

Control Flow.
-------------

Ada comes with a variety of control flow structures. The two we’ll be looking at today are :ada:`if` statements and :ada:`for` loops.

**If Statements.**
If statements are useful for logical branching of code.

.. code-block:: ada

    if condition then
      statement;
    elsif condition2 then
      statement2;
    else
      statement3;
    end if;

**For Loops.**
You can use for loops to iterate over ranges of objects or arrays.

.. code-block:: ada

    for I in Integer range 1 .. 10 loop
      statement
    end loop For_Loop;

Procedures vs. Functions.
-------------------------

Functions & procedures group together code to perform a task. For example, you could write a function to add numbers:

.. code:: ada run_button

    with Ada.Text_IO; use Ada.Text_IO;

    procedure Main is

      function Add (A : Integer; B : Integer) return Integer is
      begin
        return A + B;
      end Add;

    begin
      Put_Line ("3 + 5 = " & Integer'Image(Add(3, 5)));
    end Main;

Procedures are functions that don’t return any value. They can alter the parameters that are passed in though.

Ada is so much more...
----------------------

This is just the tip of the iceberg.  Ada is very mature, modern language with lots of useful features.

**Ada also includes...**
#. Object Oriented Programming
#. Generic Objects
#. Concurrency
#. Packages
#. And so much more!

.. todo:: insert mountain picture

Meet SPARK Ada.
---------------

.. admonition:: Key Term

  **SPARK Ada:** A subset of the Ada programming language, designed for program verification.  In this workshop we’ll be using SPARK Ada.

.. admonition:: Key Term

  **Program Verification:** A program that checks that source code is well formed and it performs as intended and is bug free.

.. todo:: insert SPARK banner logo

**SPARK Ada checks for...**
* **Syntax Errors** - Is the code properly formatted & will it compile?
* **Run-time Errors** - No exceptions, no buffer overflows, no division by zero, etc.
* **Proof of Properties** - True or false expressions that you write and the compiler verifies.

How does SPARK find bugs?
-------------------------

SPARK Ada’s tools analyze your code for exceptions & erroneous behavior.

.. code-block:: ada

    My_Array (Index) := (X * Y) / Z;

**Questions SPARK asks...**
* Is :ada:`Index` in the bounds of :ada:`My_Array`?
* Could :ada:`(X * Y) / Z` be potentially out of range?
* Could :ada:`(X * Y) / Z`  potentially cause an overflow?
* Is :ada:`Z` potentially equal to :ada:`0`?
* Are :ada:`My_Array`, :ada:`X`, :ada:`Y`, and :ada:`Z` initialized?

A SPARK Ada example.
--------------------

Here is a function called :ada:`Inc` that takes in a small Integer and increments it by :ada:`+1`.

.. code-block:: ada

    package body Inc
      with SPARK_Mode => On
    is

      procedure Inc (X: in out Small_Int) is
      begin
        X := X + 1;
      end Inc;

    end Inc;

**What does this code do?**
* **Line 1:** Define the package body for :ada:`Inc`.
* **Line 2:** Enable SPARK mode for program verification.
* **Line 5:** Define a procedure called :ada:`MyInc` that takes in a :ada:`Small_Int` called :ada:`X` & updates it.
* **Lines 6 - 8:** Update the variable :ada:`X` to equal itself plus :ada:`1`.

What about the package specification for the Inc package?

.. code-block:: ada

    package Inc
      with SPARK_Mode => On
    is

      type Small_Int is range -128 .. 128;

      procedure Inc (X: in out Small_Int)
        with Pre => (X < Small_Int'Last), 
             Post => (X = X'Old + 1);

    end Inc;

*Lines 8 & 9 use SPARK’s proof of properties feature.*

**What does this code do?**
* **Line 1:** Define the package specification for :ada:`Inc`.
* **Line 2:** Enable SPARK mode for program verification.
* **Line 5:** Define a new type called :ada:`Small_Int` that is an Integer between :ada:`-128` and :ada:`+128`.
* **Line 7:** Define a procedure called :ada:`MyInc` that takes in a :ada:`Small_Int` called :ada:`X` and updates it.
* **Line 8:** Verify that :ada:`X` is always less than :ada:`+128` before the procedure is called.
* **Line 9:** Verify that :ada:`X`’s new value is equal to :ada:`X`’s old value plus :ada:`1` after calling the procedure.

Running the SPARK Prover.
-------------------------

When you run the SPARK prover, it will identify the specific lines in your code that could cause bugs.

.. todo:: add gnatprove output

For example, you would see the above output if you forgot to include the :ada:`Pre` condition to check that :ada:`X` is less than :ada:`+128`.
