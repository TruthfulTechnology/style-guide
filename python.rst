Python Code Style Guide
=======================

Linters
-------

- Use PyCodeStyle (**TODO**: document preferred non-default configuration)


Defining Functions
~~~~~~~~~~~~~~~~~~

Naming
~~~~~~

I use lowercase function names, with whole words separated by underscores.  I rarely shorten words or smash words together without a separating underscore.

I typically prefer to name functions with a very (even if it means putting ``get_`` or ``find_`` in front of the function name.


Calling Functions
-----------------

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


Comprehensions
--------------

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
