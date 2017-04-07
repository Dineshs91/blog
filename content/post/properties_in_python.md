+++
Description = ""
Tags = []
date = "2017-04-06T18:57:53+05:30"
title = "Understanding @property in python"

+++

"`@property` what is it and how does it help" is what I will be exploring in this blog.

OOP's best practices say to always use getters and setters. I will explain the reason for this in a bit.

<!--more-->

First lets look at the code

```
class Animal:
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

tiger = Animal("tiger")
tiger.name
```

`tiger.name` will print the name of the animal, which in this case is **tiger**. Now a question might arise asking, why property in the first place. Can't
I just use the below ?

```
self.name = name # In init function

tiger.name
```

To answer this question, we have to understand some properties of Object Oriented Programming. The property we are looking for is encapsulation.
Encapsulation says that the internal state of an object should be restricted in how it is accessible to the outside world. This is achieved usually
with getters and setters.


```
class Animal:
    def __init__(self):
        self._name = None

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, val)
        self._name = val

tiger = Animal()
tiger.name = "tigress" # Set the name of the animal.
```

Another use of property

```
class Processor:
    def __init__(self, dictionary):
        self.dictionary = dictionary

    @property
    def id(self):
        return dictionary.get('id')
```

What this allows us to do is to access `id` as an  instance of the object instead of the usual function.

```
dictionary = {
    "id": "381298"
}
processor = Processor(dictionary)
processor.id # This prints the id of the processor.
```

If we didn't use property the code will look like this

```
class Processor:
    def __init__(self, dictionary):
        self.dictionary = dictionary

    def id(self):
        return dictionary.get('id')
```

```
dictionary = {
    "id": "381298"
}
processor = Processor(dictionary)
processor.id() # Can be accessed as a function.
```
Getters and setters allows to modify the value of the attribute without having to change the code in all the places where it is being accessed.

Ok, we understand the use of getters and setters. Time to answer this question "But I still don't get it why I should use property".
Lets take an example

```
class Cell(object):
    . . .
    def getvalue(self, obj):
        "Recalculate cell before returning value"
        self.recalc()
        return obj._value
    value = property(getvalue)
```
Example taken from [python descriptor doc](https://docs.python.org/2/howto/descriptor.html#properties)

What this achieves is whenever we access `new Cell().getvalue` the method `self.recalc()` is always called. Quoting the docs "Calling property() is a 
succinct way of building a data descriptor that triggers function calls upon access to an attribute".

There is no way in python for restricting access to attributes. The clean way of achieving something similar is using property.

## References
https://docs.python.org/3/howto/descriptor.html#properties