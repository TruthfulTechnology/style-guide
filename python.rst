Python Code Style Guide
=======================

Linters
-------

- Use PyCodeStyle (**TODO**: document preferred non-default configuration)


Defining Functions
~~~~~~~~~~~~~~~~~~

Sometimes you need to write your own functions.

Naming
~~~~~~

I use lowercase function names, with whole words separated by underscores.  I rarely shorten words or smash words together without a separating underscore.

I typically prefer to name functions with a very (even if it means putting ``get_`` or ``find_`` in front of the function name.


Line Wrapping
~~~~~~~~~~~~~

I tend to wrap function definitions with many arguments like this:

.. code-block:: python

    def function_with_many_args(first_arg, second_arg, third_arg,
                                fourth_arg, optional_arg1=None,
                                optional_arg2=None, *, keyword_arg1,
                                keyword_arg2, keyword_arg3):

Note that this style differs from the style I use for calling functions with many arguments.

I do not use a special notation to distinguish positional arguments, arguments with default values, or keyword-only arguments in function definitions.


Arguments
~~~~~~~~~

I prefer to limit the number of arguments my functions accept.  If a function accepts more than a couple arguments, I usually prefer to make some or all arguments keyword only:

.. code-block:: python

    def function_with_many_args(first_arg, second_arg, *, keyword_arg1=None,
                                keyword_arg2=None, keyword_arg3=None):

I prefer not to write functions that require more than a few arguments.  I see many required arguments is an indication that there's a missing collection/container/data type.


Calling Functions
-----------------

What good is defining a function if you never call it?

Spacing
~~~~~~~

I do not use whitespace before the opening parenthesis of a function call nor inside the parenthesis of a function call:

.. code-block:: python

    def __str__(self):
        return " ".join((self.first_name, self.last_name))

I never do this:

.. code-block:: python

    def __str__(self):
        return " ".join ((self.first_name, self.last_name))

and I never do this:

.. code-block:: python

    def __str__(self):
        return " ".join( (self.first_name, self.last_name) )


Line Wrapping
~~~~~~~~~~~~~

When line-wrapping a function call that includes all keyword arguments, **I prefer the following code style**:

.. code-block:: python

    def __repr__(self):
        return "{class_name}({first_name}, {last_name}, {age})".format(
            class_name=type(self).__name__,
            first_name=repr(self.first_name),
            last_name=repr(self.last_name),
            age=self.age,
        )

I put the opening parenthesis at the end of the first line and the closing parenthesis on its own line aligned with the beginning of the initiating line.  Each keyword argument goes on its own line which ends in a comma, including the final one.  The keyword arguments are indented 4 spaces (one indentation level) from the initiating line.

I prefer not to put the closing parenthesis on the same line as the final keyword argument:

.. code-block:: python

    def __repr__(self):
        return "{class_name}({first_name}, {last_name}, {age})".format(
            class_name=type(self).__name__,
            first_name=repr(self.first_name),
            last_name=repr(self.last_name),
            age=self.age)

I also do not like to see multiple arguments on one line:

.. code-block:: python

    def __repr__(self):
        return "{class_name}({first_name}, {last_name}, {age})".format(
            class_name=type(self).__name__, first_name=repr(self.first_name),
            last_name=repr(self.last_name), age=self.age)

I also prefer not to adhere to this (also very common) code style:

.. code-block:: python

    def __repr__(self):
        return "{cls}({first}, {last}, {age})".format(cls=type(self).__name__,
                                                      first=repr(self.first_name),
                                                      last=repr(self.last_name),
                                                      age=self.age)


Looping
-------

While Loops
~~~~~~~~~~~

I use ``while`` loops very rarely.  If I need an infinite loop, I'll use ``while True``:

.. code-block:: python

    while True:
        print("do something forever")

Typically if I find I'm using a ``while`` loop, I'll consider whether I could either:

1. Rewrite the loop as a ``for`` loop
2. Create a generator function that hides the ``while`` loop and loop over the generator with a ``for`` loop


Looping with Indexes
~~~~~~~~~~~~~~~~~~~~

I never want to see this in my code:

.. code-block:: python

    for i in range(len(colors)):
        print(colors[i])

If I ever see ``range(len(colors))``, I consider whether I actually need an index.

If I'm using an index to loop over multiple lists at the same time, I'll use ``zip``:

.. code-block:: python

    for color, ratio in zip(colors, ratios):
        print("{}% {}".format(ratio * 100, color))

If I do really need an index, I'll use ``enumerate``:

.. code-block:: python

    for num, name in enumerate(presidents, start=1):
        print("President {}: {}".format(num, name))


Embrace Comprehensions
~~~~~~~~~~~~~~~~~~~~~~

Whenever I have a loop that converts one iterable into another, I try to convert it to a comprehension instead.

This is how I usually start:

.. code-block:: python

    doubled_odds = []
    for n in numbers:
        if n % 2 == 1:
            doubled_odds.append(n)

This is what I prefer to refactor that to:

.. code-block:: python

    doubled_odds = [
        n * 2
        for n in numbers
        if n % 2 == 1
    ]

If I can think up a way to rewrite a loop as mapping an iterable to an iterable, I will attempt to do so and see whether I like the output.


Comprehensions
--------------

I like list comprehensions.

Line Wrapping
~~~~~~~~~~~~~

I prefer to write list comprehensions, set comprehensions, dictionary comprehensions, and generator expressions on multiple lines.

I like to add line breaks between the mapping, looping, and (optional) conditional parts of a comprehension:

.. code-block:: python

    doubled_odds = [
        n * 2
        for n in numbers
        if n % 2 == 1
    ]

I do not like to wrap my comprehensions in places besides between the three parts:

.. code-block:: python

    doubled_odds = [
        n * 2 for n
        in numbers if
        n % 2 == 1
    ]

My preferred wrapping style for list comprehensions is very similar to the style I prefer for wrapping function calls.

I wrap dictionary comprehensions like this:

.. code-block:: python

    flipped = {
        value: key
        for key, value in original.items()
    }

I prefer to wrap comprehensions with multiple ``for`` clauses like this:

.. code-block:: python

    flattened = [
        n
        for row in matrix
        for n in row
    ]

When I use generator expressions inside a function call, I only use one set of parenthesis and I prefer to wrap them over multiple lines:

.. code-block:: python

    sum_of_squares = sum(
        n ** 2
        for n in numbers
    )


For a very short comprehension, I often find it acceptable to use just one line of code:

.. code-block:: python

    sum_of_squares = sum(n**2 for n in numbers)

I almost always use multiple lines when there's an conditional section or when the mapping or looping sections are not very short.
