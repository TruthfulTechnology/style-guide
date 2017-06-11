Python Code Style Guide
=======================

Baseline:

- I try to abide by strong recommendations in `PEP 8 <https://www.python.org/dev/peps/pep-0008/>`_
- I use `Flake8 <http://flake8.pycqa.org>`_ for some automated style checks


Variables
---------

Sometimes you need to name things.


Naming
~~~~~~

I tend to use long variable names: whole words and often multiple words.

I will sometimes use a single letter variable name for a looping variable, but usually I'll try to use a longer name even if it means I need to split the line up for readability.


Naming Indexes
~~~~~~~~~~~~~~

Whenever I see something like ``some_variable[0]`` or ``some_variable[2]``, I treat this as an indication that I should be relying on iterable unpacking.

Instead of this:

.. code-block:: python

    do_something(things[0], things[1])

I'd rather see this:

.. code-block:: python

    first, second = things
    do_something(first, second)

Instead of this:

.. code-block:: python

    do_something(things[0], things[1:-1], things[-1])

I'd rather see this:

.. code-block:: python

    head, *middle, tail = things
    do_something(head, middle, tail)


Unused Variables
~~~~~~~~~~~~~~~~

I try to avoid making variables I'll never use.

There are two times I sometimes find I need to make a variable even though I'll never use it: iterable unpacking and a list comprehension over a ``range``:

.. code-block:: python

    head, *unused_middle, tail = things
    do_something(head, tail)
    matrix = [[0] * 3 for unused_index in range(3)]

I tend to prefer using ``_`` for these variables which are never used:

.. code-block:: python

    head, *_, tail = things
    do_something(head, tail)
    matrix = [[0] * 3 for _ in range(3)]


Compacting Assignments
~~~~~~~~~~~~~~~~~~~~~~

I sometimes use iterable unpacking to compact multiple assignment statements onto one line.  I only do this when the assignments are very tightly related:

.. code-block:: python

    word1, word2 = word1.upper(), word2.upper()
    x, y, z = (a1 - a2), (b1 - b2), (c1 - c2)


Defining Functions
------------------

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


Conditionals
------------

I do not use parenthesis around conditional expressions in ``if`` statements unless they wrap over multiple lines.


Inline If Statements
~~~~~~~~~~~~~~~~~~~~

Consider using inline ifs if assigning to or returning two things.

Instead of this:

.. code-block:: python

    if name:
        greeting = "Hello {}".format(name)
    else:
        greeting = "Hi"

Consider using this:

.. code-block:: python

    greeting = "Hello {}".format(name) if name else "Hi"

Also consider splitting inline ``if`` statements over multiple lines for improved readability:

.. code-block:: python

    greeting = (
        "Hello {}".format(name)
        if name
        else "Hi"
    )


Truthiness
~~~~~~~~~~

Instead of checking emptiness through length or other means:

.. code-block:: python

    if len(results) == 0:
        print("No results found.")

    if len(failures) > 0:
        print("There were failures during processing.")

Rely on truthiness to check for emptiness:

.. code-block:: python

    if not results:
        print("No results found.")

    if failures:
        print("There were failures during processing.")

Do not rely on truthiness for checking zeroness or non-zeroness though.

Instead of this:

.. code-block:: python

    if n % 2:
        print("The given number is odd")

    if not step_count:
        print("No steps taken.")

Do this:

.. code-block:: python

    if n % 2 == 1:
        print("The given number is odd")

    if step_count == 0:
        print("No steps taken.")


Conversion to bool
~~~~~~~~~~~~~~~~~~

If you ever see code that sets a variable to ``True`` or ``False`` based on a condition:

.. code-block:: python

    if results:
        found_results = True
    else:
        found_results = False

    if not failures:
        success = True
    else:
        success = False

Rely on truthiness by converting the condition to a ``bool`` instead, either explicitly for the truthy case or implicitly using ``not`` for the falsey case:

.. code-block:: python

    found_results = bool(results)

    success = not failures

Keep in mind that sometimes no conversion is necessary.

The condition here is already a boolean value:

.. code-block:: python

    if n % 2 == 1:
        is_odd = True
    else:
        is_odd = False

So type-casting to a ``bool`` would be redundant.  Instead simply set the variable equal to the expression:

.. code-block:: python

    is_odd = (n % 2 == 1)


Long if-elif chains
~~~~~~~~~~~~~~~~~~~

Python doesn't have switch statements.  Instead, you'll often see Python developers use an ``if`` statement with many ``elif`` statements.

.. code-block:: python

    if n == "zero":
        numbers.append(0)
    elif n == "one":
        numbers.append(1)
    elif n == "two":
        numbers.append(2)
    elif n == "three":
        numbers.append(3)
    elif n == "four":
        numbers.append(4)
    elif n == "five":
        numbers.append(5)
    elif n == "six":
        numbers.append(6)
    elif n == "seven":
        numbers.append(7)
    elif n == "eight":
        numbers.append(8)
    elif n == "nine":
        numbers.append(9)
    else:
        numbers.append(' ')

Instead of using many ``elif`` statements, consider using a dictionary.  This alternative is often (but not always) possible.

.. code-block:: python

    words_to_digits = {
        'zero': 0,
        'one': 1,
        'two': 2,
        'three': 3,
        'four': 4,
        'five': 5,
        'six': 6,
        'seven': 7,
        'eight': 8,
        'nine': 9,
    }
    numbers.append(translation.get(n, " "))


Strings
-------

In Python 3.6, I use f-strings for combining multiple strings.

In Python 2.7 and Python 3.5, I use the ``format`` method for string formatting.  I never use ``%`` to format strings.

I usually prefer f-strings or the ``format`` method over string concatenation.

If I am joining a list of values together, I use the ``join`` method instead.

For string literals with line breaks in them, I often prefer to use a multi-line string combined with ``textwrap.dedent``.  I may occasionally use ``'\n'.join()`` instead.


Regular Expressions
-------------------

Avoid using regular expressions if there's a simpler and equally accurate way of expressing your target search/transformation.

Unless your regular expression is extremely simple, always use a multi-line string and ``VERBOSE`` mode when representing your regular expression.


Flake8 Customizations
---------------------

I install `flake8 <http://flake8.pycqa.org>`_, `pep8-naming <https://github.com/PyCQA/pep8-naming>`_, `flake8-import-order <https://github.com/PyCQA/flake8-import-order>`_, `flake8-bugbear <https://github.com/PyCQA/flake8-bugbear>`_, and `flake8-docstrings <https://github.com/PyCQA/flake8-docstrings>`_:

.. code-block:: bash

    $ pip install flake8 pep8-naming flake8-import-order flake8-bugbear flake8-docstrings

I use this Flake8 configuration::

    [flake8]
    ignore =
        N806,   # Variables can be CamelCase
        D1      # Don't require docstrings
    max-complexity = 10
