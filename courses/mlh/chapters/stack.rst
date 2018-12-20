.. meta::
  :author: AdaCore

:next_state: False

The Stack Data Structure
========================

:code-config:`reset_accumulator=True`

.. role:: ada(code)
   :language: ada

What are we building today?
---------------------------

**We're Building a Stack!**

* We’re going to build a common data structure called a “Stack”.
* The below sample code is mostly working with a few bugs.
* We’ll use some of SPARK Ada’s cool features to find and fix the issues.

.. todo:: add stack graphics

So, what is a Stack?
--------------------

A Stack is like a pile of dishes...

#. The pile starts out empty.
#. You add (“push”) a new plate (“data”) to the Stack by placing it on the top of the pile.
#. To get plates (“data”) out, you take the one off the top of the pile (“pop”).
#. Our stack has a maximum height (“size”) of 9 dishes.

.. todo:: add plate graphic

Pushing items onto the stack.
-----------------------------

Here’s what should happen if we pushed the string :ada:`MLH` onto the stack.


Step 0: Empty List

+----------+
|1:        |
+----------+
|2:        |
+----------+
|3:        |
+----------+
|4:        |
+----------+
|5:        |
+----------+

:ada:`Last = 0`

Step 1: :ada:`Push ("M")`

+----------+
|1:   M    |
+----------+
|2:        |
+----------+
|3:        |
+----------+
|4:        |
+----------+
|5:        |
+----------+

:ada:`Last = 1`

Step 2: :ada:`Push ("L")`

+----------+
|1:   M    |
+----------+
|2:   L    |
+----------+
|3:        |
+----------+
|4:        |
+----------+
|5:        |
+----------+

:ada:`Last = 2`

Step 3: :ada:`Push ("H")`

+----------+
|1:   M    |
+----------+
|2:   L    |
+----------+
|3:   H    |
+----------+
|4:        |
+----------+
|5:        |
+----------+

:ada:`Last = 3`

Step 4: :ada:`Top`

+----------+
|1:   M    |
+----------+
|2:   L    |
+----------+
|3:   H    |
+----------+
|4:        |
+----------+
|5:        |
+----------+

:ada:`Last = 3`
returns: :ada:`H`

The list starts out empty.  Each time we push a character onto the stack, :ada:`Last` increments by :ada:`1`.

**Popping items from the stack**

Here’s what should happen if we popped :ada:`2` characters off our stack & then clear it.

Step 0: Starting List

+----------+
|1:   M    |
+----------+
|2:   L    |
+----------+
|3:   H    |
+----------+
|4:        |
+----------+
|5:        |
+----------+

:ada:`Last = 3`

Step 1: :ada:`Pop`

+----------+
|1:   M    |
+----------+
|2:   L    |
+----------+
|3:   H    |
+----------+
|4:        |
+----------+
|5:        |
+----------+

:ada:`Last = 2`
returns: :ada:`H`

Step 2: :ada:`Pop`

+----------+
|1:   M    |
+----------+
|2:   L    |
+----------+
|3:   H    |
+----------+
|4:        |
+----------+
|5:        |
+----------+

:ada:`Last = 1`
returns: :ada:`L`

Step 3: :ada:`Clear`

+----------+
|1:   M    |
+----------+
|2:   L    |
+----------+
|3:   H    |
+----------+
|4:        |
+----------+
|5:        |
+----------+

:ada:`Last = 0`

Note that :ada:`Pop` & :ada:`Clear` don’t unset the Storage array’s elements, they just change the value of Last.

Continue to the :ref:`Labs Section <mlh_labs>` to continue with the workshop.  