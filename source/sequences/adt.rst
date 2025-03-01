=====================
 Abstract Data Types
=====================

:Lecture: Lecture 2.1 :download:`(slides) <_static/adt/adt.pptx>`
:Objectives: Understand what is an abstract data type
:Concepts: Data, Computation, Computational Problem, Algorithm, Data Structure

So far we have used only numbers, but in real-life, we need a little
more. We want text, images, colors, sounds, dates, 2D points, users
records, etc. In this lecture, we will look at data types, what they are
and how they can help use design and reason on algorithms.

Say we are task with converting an color image to grayscale. This sounds
pretty remote from what our RAM can do? How can we write an algorithm
that does that?

- How to represent more than just numbers with machine’s symbols?

- How to move beyond int, float, characters and define our own custom
  composite data types

- How to leverage data types in algorithms

Data Types
==========

.. margin::

   .. figure:: _static/adt/images/data_type.svg
      :name: sequences/adt/data_type

      The two parts of a data types: The programming interface and the
      internal representation.

So far, we have manipulated only integers, because our RAM use Arabic
digits. What if we need the machine to manipulate other type of data,
say an image for instance? Well, from the machine standpoint, there is
nothing to do: Machines only crunch symbols. At the programming language
level however, we need new *data types*.

As shown on:numref:`sequences/adt/data_type`, a *data type* defines two
things: A symbolic representation and a *programming interface*. The
symbolic representation decides how we use the machine’s symbols to
represent the data of interest (the image in our previous example). The
programming interface includes all the procedures we use to manipulate
this new data type. For images these could be resizing, cropping,
blurring, switching to gray scales, etc.

.. important::

   A *data type* is a set of sequences of machine symbols that adhere
   to a specific representation and which we manipulate using a
   specific programming interface.

Data types come up handy for programmers because they make programs
easier to understand. Consider the following Java program that decides
whether the user answered “yes” or “no”. The keywords “boolean” and
“String” gives hints about the intention.

.. code:: java

     boolean isYes(String anwser) {
       return Character.toUpperCase(answer.charAt(0)) == 'Y';
     }

C, Pascal, Java and other *statically-typed languages* require
programmers to mark variables and parameters with the appropriate data
type. This way, the compiler can detect earlier invalid expressions such
as ``var x = 25 / 'a'``, where we attempt to divide a number by a
character. Typing can however become a hassle when they are less
obvious, and that is why *dynamically-typed languages* like Python, Ruby
or JavaScript do not enforce type-correctness and let typing mistakes
surface as runtime errors.

Symbolic Data Representation
----------------------------

.. margin::

   .. figure:: _static/adt/images/representation.svg
      :name: sequences/adt/representation

      Internal representation: Encoding and decoding data to and from
      machine symbols


The first step towards a new data type is to find a way to represent
data with the machine’s symbols. Once we agreed on a representation
format, we can extend the I/O device to translate back and forth
between data and symbols. :numref:`sequences/adt/representation` shows
how pthe representation format decides both encoding and decoding.

Encoding Data into Symbols
^^^^^^^^^^^^^^^^^^^^^^^^^^

Encodings schemes define how we represent a given piece of data using
the machine’s symbols (see
:numref:`sequences/adt/representation`). There are many encodings you
may have heard of, such as ASCII or Unicode for characters,
sign-and-magnitude or 2s-complement for integers, IEEE 754 for
floating point values, PNG for images, etc.

Take ASCII for example, the American standard for information
interchange. ASCII was developed during the 60ies to represent text for
computers and telecommunications. With ASCII, each character occupies
7 bits, so the ASCII format only accounts for :math:`128=2^7` character
symbols. That is enough anyway to capture the Latin alphabet, common
punctuation symbols, a few math symbols and more. The character ’A’
(uppercase) is represented by the number 65 or (or 41 in hexadecimal),
’B’ by 66, ’C’ by 67, etc. Lowercase letters come a bit further down
with ’a’ encoded with 97, ’b’ with 98, etc.

Decoding Data from Symbols
^^^^^^^^^^^^^^^^^^^^^^^^^^

The exact opposite of encoding—when we read data out of symbols—is known
as decoding. Returning to ASCII, decoding the four bytes ``4A-6F-68-6E``
(i.e., 74, 111, 104, 110 as decimal numbers) would be decoded as the
text “John”, as shown on :numref:`sequences/adt/representation`.

The key point about decoding is that we need to know the underlying
representation in advance. Given a sequence of symbols, one cannot say
what encoding it comes from. Take again the four bytes ``4A-6F-68-6E``
or example, they can represent:

-  The natural number as a 32-bit integer value ;

-   The real number as a 32-bit floating point value ;

-  The text “John” in ASCII ;

-  Some sort of greenish color as an RGBA color ;

-  etc.

.. admonition:: Data Types in assembly code
   :class: dropdown

   For example, our RAM manipulates only numbers in base 10 using
   Arabic digits as symbols (0, 1, 2, 3, …9).  As a result, numbers
   occurs in various roles: Some represent memory addresses (e.g., the
   ``IP`` register), some represent opcodes (e.g., 1 denotes
   ``LOAD``), and some represent actual numbers. A single sequence of
   symbols can have multiple interpretations, and in general—without
   more information—one cannot say what a bunch of symbols stands
   for. Take a single number, say 7 for example: We cannot say for
   sure if this is an address, an opcode, or just the value
   seven. This matters because if, by mistake, we use it in place of
   an opcode, the RAM would just halt.

The machine itself remains completely oblivious of such
encoding/decoding: It only transforms symbols. In our simplified RAM
architecture, encoding and decoding would take place in the I/O device:
It would convert specific representations and present data accordingly
to the user.

.. important::

   A *data structure* is the representation (i.e., the memory layout)
   chosen for a particular data type.

Programming Interface
---------------------

The second thing that characterizes a data type is its programming
interface: Procedures to manipulate the data.

Consider for the example the integer type, which comes predefined in
most programming languages (e.g., ``int`` in Java or C/C++). The integer
type comes with predefined operations that mirror the arithmetic and
logical operations that exists in mathematics (addition, subtraction,
division, modulo, comparisons, etc.), as well as conversions to other
data types such as floating point numbers or string. Some of these
operations are directly supported by the underlying machine, such as the
addition in our RAM, others require dedicated procedures.

Character is another example. In Java, the associated programming
interface includes procedures such as ``isLetter``, ``isDigit``,
``isWhiteSpace``, ``isUpperCase``, ``isLowerCase``, ``toLowerCase``,
and many more. These procedures defines what one can do with a
character, and in turn, what a character is. As shown on
:numref:`sequences/adt/data_type`, the programming interface is all
that matters to programmers, as one could possibly change internal
representation as long as the interface remains the same. Think of
programs that manipulate simple characters: The actual representation
(ASCII, Unicode, etc.) may be irrelevant.

Programming interface is what matters when it comes to algorithms and
for this course in particular.

Primitive Types
---------------

High-level programming languages such as C, Java, Python natively
supports most common data types. The compilers (or interpreter) hides
the underlying representations and expose their programming interface
through keywords, operators or standard libraries. These *primitive*
data types include

-  Boolean values (true / false), which come along with conjunction
   (and), disjunction (or), and negation (not) operators.

-  Integer values, which support both arithmetic operations as well as
   comparisons.

-  Floating point values, which also support both arithmetic operations
   and comparisons

-  Characters, often encoded either in ASCII or in Unicode.

-  Bytes (8 bits), correspond to a raw sequence of symbols

Compound Types
--------------

Primitive data types are programming languages give us to play with,
but we very often need to compose them in order to build new
“compound” data types that capture domain concepts, such as color, 2D
point, dates, time, user record, etc. Programming languages provides
three main ways to compose data types [#scott2009]_, namely structures,
arrays, and variants.

.. [#scott2009] We give here only a brief reminder, but refer to:
                *Scott, M. L. (2009). Programming language
                pragmatics. Morgan Kaufmann. Chap. 7* for a more
                comprehensive treatment

Records
^^^^^^^

(also known as structures or tuples) includes multiple entries called
*fields*, each with its own data type. Records pp resemble tuples in
mathematics which results from the Cartesian product over sets, such as
:math:`(x,y) \in T_1 \times T_2`. The key point of records is to access
fields using their name. In Pascal for example, one could describe a
player in game using the following record type

.. code:: pascal

       type Date = record
         name: string;
         score: integer;
         isCPU: boolean;
       end;

:numref:`sequences/adt/record` illustrate how the compiler may lay out
the record fields in memory. The details vary from compiler to
compiler, but the principle remains the same: Provided the record
starts at the *base address* :math:`b`, the address of the k-th field
is given by:

.. math::
   :label: eq:record

   address(f_k) = b + \sum_{i=1}^{k} size(T_i) \label{eq:record}

The compiler takes case of this and provides us with direct access to
each fields by name in constant time: Offset to all fields are
precomputed at compile time.

.. figure:: _static/adt/images/record.svg
   :name: sequences/adt/record

   A possible memory layout of a record of type :math:`T_1 \times T_2 \times T_3`


Arrays
^^^^^^

represents a sequence of items, all from the same data type. Formally,
we can think of arrays as function :math:`a` that maps integer to
specific item such :math:`a: \mathbb{N} \to T`. The key point of arrays
it to access items using their position in the sequence. Lecture 3.2
will dive into arrays. In C for example, one could represent calendar
dates using an array:

.. code:: c

       typedef int date[3];

:numref:`sequences/adt/array` shows how an array containing 3 items of
a type :math:`T` could be laid out in memory. The array is allocated
from a *base address* (:math:`b`). Since arrays contains items of a
single data type :math:`T` (whose size is known), we can deduce the
address of :math:`k`-th item using:

.. math::
   :label: eq:array

   address(k) = b + k * size(T)

This is actually the same as Equation :eq:`eq:record`, but
for a single data type. Here as well, the compiler takes care of this
and provides us access by index in constant time. This makes array the
go-to data structure for *random access*: When little is known on which
item will be accessed most often.

.. figure:: _static/adt/images/array.svg
   :name: sequences/adt/array

   A common memory layout for an array of type :math:`T^6`
   

Variants
^^^^^^^^

(also known as unions) represent a single field which can belong to
multiple data types: It can be decoded using different format. Formally,
a variant captures the union of multiple data types
:math:`T_1 \, \cup \, T_2`. We access data through the name we give to
specific interpretation. For example, in C, we could write:

.. code:: c

       union Number {
         int asInteger;
         float asFloat;
       };

Now, we can build whatever data type we please and we can combine them
using arrays, records or variants.

.. _`sec:adt`:

Abstract Data Types
===================

Data types are what we need when we create a new programming language,
but from a pure algorithmic perspective, this is not what we need. The
internal representation is irrelevant. Ideally, we do not want to write
algorithms that only apply to ASCII characters. We want algorithms that
apply to characters in general, whether they are encoded in ASCII, in
Unicode. From an algorithmic perspective, the representation is
irrelevant, and what we should focus on is the programming interface.

Unfortunately, the programming interface in itself is not enough to
define what *any* character data type ought to offer. The procedures
relates to one another in very specific ways. For instance, if I convert
a character to upper case and then convert it back to lower case, I
would get back to the same original character. So to be valid, a
character data type has to offer a set procedures that behave properly.

An *abstract data type* (ADT) [#adt-oo]_  defines the programmer’s expectations over
a programming interface, *irrespective of its representation*.
Representation is an implementation detail, and by hiding it from our
algorithms, they become more generally applicable.

.. [#adt-oo] The ideas of *information hiding*, encapsulation and ADT
             form the basis of modern object-oriented programming. See
             *Liskov, B., & Zilles, S. (1974). Programming with
             abstract data types. ACM Sigplan Notices, 9(4), 50--59*


Defining an ADT
---------------

An abstract data type defines three elements: a set of *domains*, a set
of *operations* and a set of *axioms*. Formally, an ADT defines an
algebra over the possible values of the data types. We will illustrate
each on a Boolean ADT.

Domains
^^^^^^^

The domains describe what the ADT is about, including all the data
types manipulated by the programming interface. The term “domain”
comes from mathematics, where it stands for the set of values for
which a given function is defined. For our Boolean ADT, the domain
boil down to the set of boolean values :math:`\mathbb{B}`.

Operations
^^^^^^^^^^

The operations are the procedures that we can use to manipulate our ADT.
They are often categorized into *constructor*, *queries* and *commands*.
Constructors instantiate the ADT from something else, queries convert
our ADT into something else, and commands modify it. The operations
supported by the Boolean ADT includes:

-  Constructors

   -  :math:`true: \varnothing \to \mathbb{B}` creates :math:`T`

   -  :math:`false: \varnothing \to \mathbb{B}` creates :math:`F`

-  Queries: None

-  Commands:

   -  :math:`and: \mathbb{B} \times \mathbb{B} \to \mathbb{B}`
      represents the conjunction of two Boolean values.

   -  :math:`or: \mathbb{B} \times \mathbb{B} \to \mathbb{B}` represents
      the disjunction of two Boolean values.

   -  :math:`not: \mathbb{B} \to \mathbb{B}` represents the negation.

Axioms
^^^^^^

The axioms are the relationships between the procedures that the ADT supports. In
the case of the Boolean ADT, the axioms are:

#. :math:`\forall x \in \mathbb{B}, \; and(x, false()) = false()`

#. :math:`\forall x \in \mathbb{B}, \; and(x, true()) = x`

#. :math:`not(true()) = false()`

#. :math:`not(false()) = true()`

#. :math:`\forall x,y \in \mathbb{B}^2, \; or(x, y) = not(and(not(x), not(y)))`

.. important::

   An abstract data type (ADT) captures the inherent relationships
   between the procedures that form the programming interface. It
   includes a domain, a set of operations, and a set of axioms that
   constrain the behavior of the procedures.

ADT and Correctness
-------------------

We already touched upon correctness of algorithms in :doc:`Lecture 1.3
</foundations/correctness>` and ADT is a very convenient tool for
that. Since ADT for the specification of a data type, we can use them
in two situations:

-  Thinking about the correctness of an algorithm that uses our ADT. For
   example, if we have a program that contains the following boolean
   expression :math:`or(true(), x)`, we can use the axioms of our
   Boolean ADT as follows:

   .. math::
      \begin{align}
      \forall x \in \mathbb{B}, or(true(), x) &= not(and(not(true()), not(x)) \tag{Axiom 5} \\
                                              &= not(and(false(), not(x)) \tag{Axiom 3} \\
                                              &= not(false()) \tag{Axiom 1} \\
                                              &= true() \tag{Axiom 4}
      \end{align}

   ADTs make explicit what we assume to be true when we design
   algorithms and thus greatly simplify reasoning about correctness.

-  Thinking about the correctness of an algorithm that implements the
   programming interface of our ADT. In this case, our axioms must be
   established: either proven or tested. Thinking in terms of ADT is one
   possible way to identify relevant test cases.

-  Thinking about the correctness of an implementation of our ADT.

Conclusion
==========

We saw what is a data type, and how we can define new data types either
primitive data types that encode specific type of data into machine
symbols, or composite data types that recombines existing ones using
*records*, *arrays* or *variants*.

We also saw how to abstract away the underlying machine representation
in order to define abstract data types. In the rest of this course, we
will explore well-known ADTs for sequence, sets, trees, etc. We will
look at alternative “representations” and see how and why they yield
different efficiency trade-offs.
