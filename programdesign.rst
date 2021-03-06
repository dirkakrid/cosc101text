**************
Program design
**************

Why functions?
--------------

It may not be clear why it is worth the trouble to divide a program into
functions. There are several reasons:

-  Creating a new function gives you an opportunity to name a group of
   statements, which makes your program easier to read and debug.

-  Functions can make a program smaller by eliminating repetitive code.
   Later, if you make a change, you only have to make it in one place.

-  Dividing a long program into functions allows you to test and debug
   the parts one at a time and then assemble them into a working whole.

-  Well-designed functions are often useful for many programs. Once you
   write and debug one, you can reuse it.

Solving a problem by writing and composing functions also lends itself
well to a fundamental problem solving tactic: **divide and conquer**.
The basic idea is to break down a problem into smaller, and more easily
solved problems, until each subproblem can be solved directly. Many
problems can be complex and difficult to try to solve as a whole, but if
they are broken down into smaller subtasks, hopefully the subtasks are
more manageable.

The main challenge with a divide-and-conquer approach to problem solving
is *how to decompose the problem*: it isn't always apparent how to break
a problem down into smaller components. As we work through more
challenging problems, we will gain practice with this technique and
learn different strategies for applying it.

Incremental development
-----------------------

Assuming you've figured out how to break down a problem into smaller
subtasks, you are still faced with the challenge of writing larger and
more complex functions. As a result, you might find yourself spending
more time debugging.

To deal with writing increasingly complex programs, you might want to
try a process called **incremental development**. The goal of
incremental development is to avoid long debugging sessions by adding
and testing only a small amount of code at a time.

As an example, suppose you want to find the distance between two points,
given by the coordinates :math:`(x_1, y_1)` and :math:`(x_2, y_2)`. By
the Pythagorean theorem, the distance is:

.. math::

   \mathrm{distance} = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}

The first step is to consider what a ``distance`` function should look
like in Python. In other words, what are the inputs (parameters) and
what is the output (return value)?

In this case, the inputs are two points, which you can represent using
four numbers. The return value is the distance, which is a
floating-point value.

Already you can write an outline of the function:

.. code-block:: python

    def distance(x1, y1, x2, y2):
        return 0.0

Obviously, this version doesn’t compute distances; it always returns
zero. But it is syntactically correct, and it runs, which means that you
can test it before you make it more complicated.

To test the new function, call it with sample arguments:

.. code-block:: python

    >>> distance(1, 2, 4, 6)
    0.0

I chose these values so that the horizontal distance is 3 and the
vertical distance is 4; that way, the result is 5 (the hypotenuse of a
3-4-5 triangle). When testing a function, it is useful to know the right
answer.

At this point we have confirmed that the function is syntactically
correct, and we can start adding code to the body. A reasonable next
step is to find the differences :math:`x_2 - x_1` and :math:`y_2 - y_1`.
The next version stores those values in temporary variables and prints
them.

.. code-block:: python

    def distance(x1, y1, x2, y2):
        dx = x2 - x1
        dy = y2 - y1
        print 'dx is', dx
        print 'dy is', dy
        return 0.0

If the function is working, it should display ``'dx is 3'`` and
``'dy is 4'``. If so, we know that the function is getting the right
arguments and performing the first computation correctly. If not, there
are only a few lines to check.

Next we compute the sum of squares of ``dx`` and ``dy``:

.. code-block:: python

    def distance(x1, y1, x2, y2):
        dx = x2 - x1
        dy = y2 - y1
        dsquared = dx**2 + dy**2
        print 'dsquared is: ', dsquared
        return 0.0

Again, you would run the program at this stage and check the output
(which should be 25). Finally, you can use ``math.sqrt`` to compute and
return the result:

.. code-block:: python

    def distance(x1, y1, x2, y2):
        dx = x2 - x1
        dy = y2 - y1
        dsquared = dx**2 + dy**2
        result = math.sqrt(dsquared)
        return result

If that works correctly, you are done. (Even better, you could construct
additional test cases to verify that the function *really* works.)
Otherwise, you might want to print the value of ``result`` before the
return statement.

The final version of the function doesn’t display anything when it runs;
it only returns a value. The ``print`` statements we wrote are useful
for debugging, but once you get the function working, you should remove
them. Code like that is called **scaffolding** because it is helpful for
building the program but is not part of the final product.

When you start out, you should add only a line or two of code at a time.
As you gain more experience, you might find yourself writing and
debugging bigger chunks. Either way, incremental development can save
you a lot of debugging time.

The key aspects of the process are:

1. Start with a working program and make small incremental changes. At
   any point, if there is an error, you should have a good idea where it
   is.

2. Use temporary variables to hold intermediate values so you can
   display and check them.

3. Once the program is working, you might want to remove some of the
   scaffolding or consolidate multiple statements into compound
   expressions, but only if it does not make the program difficult to
   read.

    **Example**:

    1. Use incremental development to write a function called
       ``hypotenuse`` that returns the length of the hypotenuse of a
       right triangle given the lengths of the two legs as arguments.
       Record each stage of the development process as you go.

Composition
-----------

As you should expect by now, you can call one function from within
another.  This ability is called **composition**.

As an example, we’ll write a function that takes two points, the center
of the circle and a point on the perimeter, and computes the area of the
circle.

Assume that the center point is stored in the variables ``xc`` and ``y`c`,
and the perimeter point is in ``xp`` and ``yp``. The first step is to find
the radius of the circle, which is the distance between the two points.
We just wrote a function, ``distance``, that does that:

.. code-block:: python

    radius = distance(xc, yc, xp, yp)

The next step is to find the area of a circle with that radius; we just
wrote that, too:

.. code-block:: python

    result = area(radius)

Encapsulating these steps in a function, we get:

.. code-block:: python

    def circle_area(xc, yc, xp, yp):
        radius = distance(xc, yc, xp, yp)
        result = area(radius)
        return result

The temporary variables ``radius`` and ``result`` are useful for development
and debugging, but once the program is working, we can make it more
concise by composing the function calls:

.. code-block:: python

    def circle_area(xc, yc, xp, yp):
        return area(distance(xc, yc, xp, yp))


Turtles
-------

In this section, we'll use the ``turtle`` module which is built in to
Python as a way to get more practice with program design concepts. The
``turtle`` module provides a fairly simple way to draw on the screen.
Here is an example to get started:

.. code-block:: python

    import turtle

    def main():
        turtle.forward(100)  # move forward 100 units
        turtle.left(90)      # turn left 90 degrees

        turtle.forward(100)  # forward 100 units
        turtle.left(90)      # left 90 degrees

        turtle.forward(100)  # forward 100 units
        turtle.left(90)      # left 90 degrees

        turtle.forward(100)  # forward 100 units
        turtle.left(90)

        turtle.done()        # all done!

    main()

The following screen shot shows the result of running the program. We
first import the ``turtle`` module (which is built in to all Python
versions). After that, we have a ``main`` function inside which we
include all our turtle drawing statements. The ``forward`` function
takes one parameter, which is the number of units to move forward. The
``left`` function takes a number of degrees to turn to the left. A
turtle starts out in the middle of the screen, facing to the right. If
you carefully read the above program, you'll see that we have the turtle
draw each side of a square. At the end, the turtle is left facing
directly right again.

.. figure:: figs/turtle.png
   :align: center
   :alt: Turtle window after running the example program

   Turtle window after running the example program

The ``turtle`` module contains other functions to steer the turtle
around the screen, including ``backward``, ``right``, ``setposition``,
and ``setheading``. The Python ``turtle`` documentation has all the
details on these functions: http://docs.python.org/library/turtle.html.

Also, each turtle is holding a pen, which is either down or up; if the
pen is down, the turtle leaves a trail when it moves. The functions
``penup`` and ``pendown`` can control whether the pen is up or down.
There is also a ``pencolor`` function that controls the color of the
pen.

At the end of any ``turtle`` program, you should always call the
``done`` function. If you do not call this function, you may need to
restart IDLE after running a program that uses ``turtle``. (Different
operating systems behave differently in this regard. Especially on
Windows systems, you should remember to call ``turtle.done()`` at the
end of a turtle drawing program.)

    **Exercises**:

       First, see if you can simplify the above program by using a ``for`` loop
       to draw each side of the square.

       Now, consider the following series using ``turtle``. They are meant
       to be fun, but they have a point, too. While you are working on them,
       think about what the point is.

       The sections that immediately follow have solutions to the exercises, so
       don’t look until you have finished (or at least tried).

        1. Write a function called ``square`` that uses ``turtle`` to draw a
           square.

        2. Add a parameter, named ``length``, to ``square``. Modify the
           function so length of the sides is ``length``, and then modify
           the function call to provide an argument for the length. Run the
           program again. Test your program with a range of values for
           ``length``.

        3. Make a copy of ``square`` and change the name to ``polygon``. Add
           another parameter named ``n`` and modify the body so it draws an
           n-sided regular polygon.
    
           Hint: instead of passing 90 to the ``left`` or ``right`` function
           for turning the turtle, you'll need to specify a different value.
           As another hint, the exterior angles of an n-sided regular
           polygon are :math:`360.0 / n` degrees.
    
        4. Write a function called ``circle`` that takes a radius, ``r``, as
           a parameter and that draws an approximate circle by invoking
           ``polygon`` with an appropriate length and number of sides. Test
           your function with a range of values of ``r``.

           Hint: figure out the circumference of the circle and make sure
           that ``length * n = circumference``.

           Another hint: if the turtle drawing is too slow for your taste,
           you can call ``turtle.speed('fastest')``.

        5. Make a more general version of ``circle`` called ``arc`` that
           takes an additional parameter ``angle``, which determines what
           fraction of a circle to draw. ``angle`` is in units of degrees,
           so when ``angle=360``, ``arc`` should draw a complete circle.

Encapsulation
-------------

The first exercise asks you to put your square-drawing code into a
function definition and then call the function, passing the turtle as a
parameter. Here is a solution:

.. code-block:: python

    import turtle

    def square():
        for i in range(4):
            turtle.forward(100)
            turtle.left(90)

    square()
    turtle.done()

The innermost statements, ``forward`` and ``left`` are indented twice to
show that they are inside the ``for`` loop, which is inside the function
definition. The next line, ``square()``, is flush with the left margin,
so that is the end of both the ``for`` loop and the function definition.

The ``for`` loop above is a bit odd in the sense that we never use the
variable ``i`` inside the statement body. This isn't uncommon in
situations in which we want a statement body to be repeated a specific
number of times, but we don't necessarily have to keep track of which
iteration we're on.

Wrapping a piece of code up in a function is called **encapsulation**.
One of the benefits of encapsulation is that it attaches a name to the
code, which serves as a kind of documentation. Another advantage is that
if you re-use the code, it is more concise to call a function twice than
to copy and paste the body!

Generalization
--------------

The next step is to add a ``length`` parameter to ``square``. Here is a
solution:

.. code-block:: python

    import turtle

    def square(length):
        for i in range(4):
            turtle.forward(length)
            turtle.left(90)

    square(100)
    turtle.done()

Adding a parameter to a function is called **generalization** because it
makes the function more general: in the previous version, the square is
always the same size; in this version it can be any size.

The next step is also a generalization. Instead of drawing squares,
``polygon`` draws regular polygons with any number of sides. Here is a
solution:

.. code-block:: python

    import turtle

    def polygon(n, length):
        angle = 360.0 / n
        for i in range(n):
            turtle.forward(length)
            turtle.left(angle)

    polygon(7, 70)
    turtle.done()

This draws a 7-sided polygon with side length 70. If you have more than
a few numeric arguments, it is easy to forget what they are, or what
order they should be in. It is legal, and sometimes helpful, to include
the names of the parameters in the argument list:

.. code-block:: python

    polygon(n=7, length=70)

These are called **keyword arguments** because they include the
parameter names as "keywords" (not to be confused with Python keywords
like ``for`` and ``def``).

This syntax makes the program more readable. It is also a reminder about
how arguments and parameters work: when you call a function, the
arguments are assigned to the parameters.

Interface design
----------------

The next step is to write ``circle``, which takes a radius, ``r``, as a
parameter. Here is a simple solution that uses ``polygon`` to draw a
50-sided polygon:

.. code-block:: python

    def circle(r):
        circumference = 2 * math.pi * r
        n = 50
        length = circumference / n
        polygon(n, length)

The first line computes the circumference of a circle with radius ``r``
using the formula :math:`2 \pi r`. Since we use ``math.pi``, we have to
import ``math``. Remember that by convention, ``import`` statements
should be put at the beginning of the script.

``n`` is the number of line segments in our approximation of a circle,
so ``length`` is the length of each segment. Thus, ``polygon`` draws a
50-sides polygon that approximates a circle with radius ``r``.

One limitation of this solution is that ``n`` is a constant, which means
that for very big circles, the line segments are too long, and for small
circles, we waste time drawing very small segments. One solution would
be to generalize the function by taking ``n`` as a parameter. This would
give the user (whoever calls ``circle``) more control, but the interface
would be less clean.

The **interface** of a function is a summary of how it is used: what are
the parameters? What does the function do? And what is the return value?
An interface is "clean" if it is "as simple as possible, but not
simpler." (Einstein)

In this example, ``r`` belongs in the interface because it specifies the
circle to be drawn. ``n`` is less appropriate because it pertains to the
details of *how* the circle should be rendered.

Rather than clutter up the interface, it is better to choose an
appropriate value of ``n`` depending on ``circumference``:

.. code-block:: python

    def circle(r):
        circumference = 2 * math.pi * r
        n = int(circumference / 3) + 1
        length = circumference / n
        polygon(n, length)

Now the number of segments is (approximately) ``circumference/3``, so
the length of each segment is (approximately) 3, which is small enough
that the circles look good, but big enough to be efficient, and
appropriate for any size circle.

Refactoring
-----------

When we wrote ``circle``, we were able to re-use ``polygon`` because a
many-sided polygon is a good approximation of a circle. But ``arc`` is
not as cooperative; we can’t use ``polygon`` or ``circle`` to draw an
arc.

One alternative is to start with a copy of ``polygon`` and transform it
into ``arc``. The result might look like this:

.. code-block:: python

    def arc(r, angle):
        arc_length = 2 * math.pi * r * angle / 360
        n = int(arc_length / 3) + 1
        step_length = arc_length / n
        step_angle = float(angle) / n

        for i in range(n):
            turtle.forward(step_length)
            turtle.left(step_angle)

The second half of this function looks like ``polygon``, but we can't
re-use ``polygon`` without changing the interface. We could generalize
``polygon`` to take an angle as a third argument, but then ``polygon``
would no longer be an appropriate name! Instead, let's call the more
general function ``polyline``:

.. code-block:: python

    def polyline(n, length, angle):
        for i in range(n):
            turtle.forward(length)
            turtle.forward(angle)

Now we can rewrite ``polygon`` and ``arc`` to use ``polyline``:

.. code-block:: python

    def polygon(n, length):
        angle = 360.0 / n
        polyline(n, length, angle)

    def arc(r, angle):
        arc_length = 2 * math.pi * r * angle / 360
        n = int(arc_length / 3) + 1
        step_length = arc_length / n
        step_angle = float(angle) / n
        polyline(n, step_length, step_angle)

Finally, we can rewrite ``circle`` to use ``arc``:

.. code-block:: python

    def circle(r):
        arc(r, 360)

This process—rearranging a program to improve function interfaces and
facilitate code re-use—is called **refactoring**. In this case, we
noticed that there was similar code in ``arc`` and ``polygon``, so we
“factored it out” into ``polyline``.

If we had planned ahead, we might have written ``polyline`` first and
avoided refactoring, but often you don’t know enough at the beginning of
a project to design all the interfaces. Once you start coding, you
understand the problem better. Sometimes refactoring is a sign that you
have learned something.

Here's the full set of code we wrote:

.. code-block:: python

    import turtle
    import math

    def polyline(n, length, angle):
        for i in range(n):
            turtle.forward(length)
            turtle.left(angle)

    def polygon(n, length):
        angle = 360.0 / n
        polyline(n, length, angle)

    def arc(r, angle):
        arc_length = 2 * math.pi * r * angle / 360
        n = int(arc_length / 3) + 1
        step_length = arc_length / n
        step_angle = float(angle) / n
        polyline(n, step_length, step_angle)

    def circle(r):
        arc(r, 360)

..


A development plan
------------------

A **development plan** is a process for writing programs. The process we
used in this case study is “encapsulation and generalization.” The steps
of this process are:

1. Start by writing a small program with no function definitions.

2. Once you get the program working, encapsulate it in a function and
   give it a name.

3. Generalize the function by adding appropriate parameters.

4. Repeat steps 1–3 until you have a set of working functions. Copy and
   paste working code to avoid retyping (and re-debugging).

5. Look for opportunities to improve the program by refactoring. For
   example, if you have similar code in several places, consider
   factoring it into an appropriately general function.

This process has some drawbacks, but it can be useful if you don’t know
ahead of time how to divide the program into functions. This approach
lets you design as you go along.

docstring
---------

A **docstring** is a string at the beginning of a function that explains
the interface (“doc” is short for “documentation”). Here is an example:

::

    def polyline(length, n, angle):
        """Draw n line segments with the given length and
        angle (in degrees) between them.  
        """    
        for i in range(n):
            turtle.forward(length)
            turtle.left(angle)

This docstring is a triple-quoted string, also known as a multiline
string because the triple quotes allow the string to span more than one
line.

It is terse, but it contains the essential information someone would
need to use this function. It explains concisely what the function does
(without getting into the details of how it does it). It explains what
effect each parameter has on the behavior of the function and what type
each parameter should be (if it is not obvious).

Writing this kind of documentation is an important part of interface
design. A well-designed interface should be simple to explain; if you
are having a hard time explaining one of your functions, that might be a
sign that the interface could be improved.

Debugging
---------

An interface is like a contract between a function and a caller. The
caller agrees to provide certain parameters and the function agrees to
do certain work.

For example, ``polyline`` requires three arguments. The first has to be
a number, and it should probably be positive, although it turns out that
the function works even if it isn’t. The second argument should be an
integer; ``range`` complains otherwise (depending on which version of
Python you are running). The third has to be a number, which is
understood to be in degrees.

These requirements are called **preconditions** because they are
supposed to be true before the function starts executing. Conversely,
conditions at the end of the function are **postconditions**.
Postconditions include the intended effect of the function (like drawing
line segments) and any side effects (like moving the turtle).

Preconditions are the responsibility of the caller. If the caller
violates a (properly documented!) precondition and the function doesn’t
work correctly, the bug is in the caller, not the function.

Note that the ``assert`` function described in the last chapter can be
incredibly helpful for verifying pre- or post-conditions.

Glossary
--------

divide and conquer:
    A problem solving strategy that proceeds by breaking down a problem
    into smaller and smaller subtasks, until a subtask can be solved
    directly.

incremental development:
    A program development plan intended to avoid debugging by adding and
    testing only a small amount of code at a time.

scaffolding:
    Code that is used during program development but is not part of the
    final version.

encapsulation:
    The process of transforming a sequence of statements into a function
    definition.

generalization:
    The process of replacing something unnecessarily specific (like a
    number) with something appropriately general (like a variable or
    parameter).

keyword argument:
    An argument that includes the name of the parameter as a "keyword."

interface:
    A description of how to use a function, including the name and
    descriptions of the arguments and return value.

refactoring:
    The process of modifying a working program to improve function
    interfaces and other qualities of the code.

development plan:
    A process for writing programs.

docstring:
    A string that appears in a function definition to document the
    function's interface.

precondition:
    A requirement that should be satisfied by the caller before a
    function starts.

postcondition:
    A requirement that should be satisfied by the function before it
    ends.

.. rubric:: Exercises

1. Write an appropriately general set of functions that can draw
   flowers like this:

.. figure:: figs/flowers.png
   :align: center
   :alt: Example flowers to draw with turtle graphics.

..

   Example flowers to draw with turtle graphics.

2. Write an appropriately general set of functions that can draw
   shapes like this:

.. figure:: figs/pies.png
   :align: center
   :alt: Example shapes to draw with turtle graphics.

..

   Example shapes to draw with turtle graphics.

3. The letters of the alphabet can be constructed from a moderate
   number of basic elements, like vertical and horizontal lines and
   a few curves. Design a font that can be drawn with a minimal
   number of basic elements and then write functions that draw
   letters of the alphabet.

   You should write one function for each letter, with names
   ``draw_a``, ``draw_b``, etc., and put your functions in a file
   named ``letters.py``.


