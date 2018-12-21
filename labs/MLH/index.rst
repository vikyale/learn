.. meta::
  :author: AdaCore

:prev_state: False
:next_state: False

:code-config:`run_button=True;prove_button=True;accumulate_code=False;lab_editor=True`

.. _mlh-labs:

MLH Localhost - Bug-Free Coding with SPARK Ada Labs
===================================================

.. container:: content-description

    Lab content associated with the MLH Localhost - Bug-Free Coding with SPARK Ada Workshop


.. raw:: html

    <a href="https://github.com/MLH/mlh-localhost-adacore"><i class="fab fa-github"></i> Download Sample Code</a><br><br>

Objective
------------

This is where the lab is explained

Task
----

This is the task to be completed.

Input Format
------------

This is the expected input format

Constraints
-----------

These are the constraints on the input

Output Format
-------------

This is the expected output format

Sample Input
------------

:ada:`TEST TEST`

Sample Output
-------------

:ada:`TEST TEST`

Explanation
-----------

This is a sample explanation of the sample

.. code:: ada spark-report-all

    package Stack with SPARK_Mode => On is

       procedure Push (V : Character)
         with Pre => not Full,
         Post => Size = Size'Old + 1;

       procedure Pop (V : out Character)
         with Pre => not Empty,
         Post => Size = Size'Old - 1;

       procedure Clear
         with Post => Size = 0;

       function Top return Character
         with Post => Top'Result = Tab(Last);

       Max_Size : constant := 9;
       --  The stack size.

       Last : Integer range 0 .. Max_Size := 0;
       --  Indicates the top of the stack. When 0 the stack is empty.

       Tab  : array (1 .. Max_Size) of Character;
       --  The stack. We push and pop pointers to Values.

       function Full return Boolean is (Last >= Max_Size);

       function Empty return Boolean is (Last < 1);

       function Size return Integer is (Last);

    end Stack;

    package body Stack
        with SPARK_Mode => On
    is
       -----------
       -- Clear --
       -----------

       procedure Clear is
       begin
          Last := Tab'First;
       end Clear;

       ----------
       -- Push --
       ----------

       procedure Push (V : Character) is
       begin
          Tab (Last) := V;
       end Push;

       ---------
       -- Pop --
       ---------

       procedure Pop (V : out Character) is
       begin
          Last := Last - 1;
          V := Tab (Last);
       end Pop;

       ---------
       -- Top --
       ---------

       function Top return Character is
       begin
          return Tab (1);
       end Top;

    end Stack;

    with Ada.Command_Line; use Ada.Command_Line;
    with Ada.Text_IO; use Ada.Text_IO;
    with Stack;       use Stack;

    procedure Example with SPARK_Mode => Off
    is
       --------------------
       -- Get_User_Input --
       --------------------

       function Get_User_Input (Arg : String) return Character is
       begin

          if Arg'Length = 1 then
             return Arg (Arg'First);
          end if;

          Put_Line ("Invalid Input, please try again!");
          raise Program_Error;
       end Get_User_Input;

       -----------
       -- Debug --
       -----------

       procedure Debug is
       begin
          Put_Line ("**************************************");

          Put_Line ("Size: " & Integer'Image(Stack.Size));
          Put_Line ("Max Size: " & Integer'Image(Stack.Max_Size));

          if not Stack.Empty then
             Put_Line ("Top: " & Stack.Top);

             Put ("Stack: [");
             for I in Stack.Tab'First .. Stack.Size loop
                Put (Stack.Tab(I) & ", ");
             end loop;
             Put_Line ("]");
          else
             Put_Line ("Top: Null");
             Put_Line ("Stack: []");
          end if;

          Put_Line ("**************************************");
       end Debug;

    begin

       ----------
       -- Main --
       ----------
       for I in 1 .. Argument_Count loop
          declare
             S : Character := Get_User_Input (Argument (I));
          begin
             if S = 'd' then
                Debug;
             elsif S = 'p' then
                if not Stack.Empty then
                   Stack.Pop (S);
                   Put_Line ("Popped: " & S);
                else
                   Put_Line ("Nothing to Pop, Stack is empty!");
                end if;
             else
                if not Stack.Full then
                   Stack.Push (S);
                   Put_Line ("Pushed: " & S);
                else
                   Put_Line ("Could not push '" & S & "', Stack is full!");
                end if;
             end if;
          end;
       end loop;
    end Example;
