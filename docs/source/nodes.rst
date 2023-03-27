Nodes
#####

Introduction
============

This module defines various types of nodes used in the input schema. These nodes represent different input elements and can be used to define custom extraction rules and examples. The nodes are organized in a hierarchical structure, with `AbstractSchemaNode` being the base class for all nodes.

AbstractSchemaNode
==================

`AbstractSchemaNode` is the base class for all schema nodes. Each node is expected to have a unique ID and a description. The ID should be unique across all inputs that belong to a given form. The description should describe what the node represents and is used during prompt generation.

ExtractionSchemaNode
====================

`ExtractionSchemaNode` is an abstract class that inherits from `AbstractSchemaNode`. It is used for inputs that involve extraction. An extraction input can be associated with extraction examples, which are 2-tuples composed of a text segment and the expected extraction.

Text
====

`Text` is a built-in input node for extracting text. It inherits from `ExtractionSchemaNode`.


Number
======

`Number` inherits from `ExtractionSchemaNode` and is used for extracting numbers. `Number` takes an `id`, `description`, `examples`, and `many` argument. 

Below is an example of the `Number` input node used in the `Trip` object:

.. code-block:: python

    schema = (
        Object(
            id="Trip",
            description="Trip information",
        ),
    )
    attributes = (
        Number(
            id="Miles",
            description="Distance of the trip in miles",
            examples=[
                ("Your destination is 100 miles away", 100),
                ("The race track is 2.5 miles long", 2.5),
                ("The 2-day trip is 200 miles long", 200),
                ("I'm only about one and a half miles out", 1.5),
            ],
            many=False

Here we've defined a `Trip` object with one attribute, `Miles`. The `Miles` attribute is a `Number` input node and provided some examples. The first and second examples are relatively straightforward; they both only contain one number in the string, but one is an `integer` and one is a `float`.

The third example adds a little more complexity. There are two numbers in the string, the trip duration and the distance. Since we're capturing the distance with this `Number` input node, we only want to extract the distance. 

Lastly, we have an example of a number written in plain English. This is a common use case for `Number` inputs. 

In each of these examples, the text explicitly states the units of the number. This is not always the case, however. In the case, you may consider combining the `Number` input node with a `Text` input node to capture the units.

Text
====

`Text` is a built-in input node for extracting text. It inherits from `ExtractionSchemaNode`.


.. code-block:: python

    schema = (
        Object(
            id="Trip",
            description="Trip information",
        ),
    )
    attributes = (
        Number(
            id="Distance",
            description="Duration of the trip in undefined units",
            examples=[
                ("Your trip will take 30 minutes", 30),
                ("We'll be back in six days", 6),
            ],
            many=False,
        ),
        Text(
            id="Units",
            description="The units of the duration",
            examples=[
                ("Your trip will take 30 minutes", "minutes"),
                ("We'll be back in six days", "days"),
            ],
            many=False,
        ),
    )



Option
======

`Option` is a built-in input node that must be part of a `Selection` input. It cannot be used independently and inherits from `AbstractSchemaNode`.

Selection
=========

`Selection` is a built-in input node representing a selection input. It inherits from `AbstractSchemaNode` and is composed of one or more `Option` inputs. A `Selection` input can be associated with extraction examples and null examples, which are segments of text for which nothing should be extracted.

Object
======

`Object` is a custom extraction input node that inherits from `AbstractSchemaNode`. It represents a complex object with multiple attributes, which can be other nodes such as `ExtractionSchemaNode`, `Selection`, or another `Object`. An `Object` input can be associated with extraction examples and null examples.
∏