+++
title = "refactoring with singleton"
author = ["felladog"]
date = 2024-05-21T11:58:00+05:45
lastmod = 2024-05-28T18:34:20+05:45
tags = ["python", "refactoring"]
categories = ["python"]
draft = false
+++

---

-   References :
    1.  <https://refactoring.guru/design-patterns/singleton> <span class="org-target" id="org-target--r1"></span>
    2.  <https://python-patterns.guide/gang-of-four/singleton/> <span class="org-target" id="org-target--r2"></span>
-   Questions :
-   Notes:

---

Welcome to tale of me writing a bad code and refactoring it with new learned knowledge to write not so bad code.

I was watching a conference video&nbsp;[^fn:1] on [metaclasses]({{< relref "20240411131135-metaprogramming.md#metaclasses" >}}), which introduced me to [singleton]({{< relref "20240521120415-design_patterns.md#singleton" >}}). I realized that i could use this creational [design pattern]({{< relref "20240521120415-design_patterns.md" >}}) [1.1](#org-target--r1) for one of my project.


## Previous Logic in my project {#previous-logic-in-my-project}

There is a factory which processes list of `Box` object to form a final `Box`. Names have been changed for their privacy.

I had a state `ProcessState` keeping tracking of whole process and is attached to all the `Box` which are being processed in this Factory. `Box` dumps data in `ProcessState` which can later be used in preparing the final box. In simple terms `ProcessState` is a global variable on which different `Box` dump their data.

```python
from typing import Tuple, Optional, Dict
from dataclasses import dataclass, field

@dataclass
class ProcessState:
    box_type: str
    state: Dict = field(default_factory=dict)
    box_count: int = 0
    log: str = ""

state = ProcessState(box_type="generic")
print(state)
```

```text
ProcessState(box_type='generic', state={}, box_count=0, log='')
```

```python
@dataclass
class Box:
    color: str = "white"
    size: Tuple[int, int] = (0, 0)
    pattern: str = "flower"
    state: Optional[ProcessState] = None

box = Box()
box
```

```text
Box(color='white', size=(0, 0), pattern='flower', state=None)
```

Creation of boxes

```python
raw_boxes = [
    Box(),
    Box(color='pink', size=(1,1), pattern="bird"),
    Box(color='green', size=(3, 3), pattern="snake"),
    Box(color='purple', size=(2, 4), pattern="dust")
]

for box in raw_boxes:
    box.state = state
    state.box_count += 1
print(boxes[0])
print(state)
```

```text
Box(color='white', size=(0, 0), pattern='flower', state=ProcessState(box_type='generic', state={}, box_count=4))
ProcessState(box_type='generic', state={}, box_count=4, log='')
```

On this list of boxes a list of processor would work on to create a final box. Each processor would have their own business logic which modifies the passed box or creates new box.

```python
def remove_snake_pattern(box: Box) -> Box:
    """
    A business logic processor that modifies and creates a new box

    Removes snake patterns and replaces with default box attribute
    """
    if box.pattern == "snake":
        new_box = Box()
        box.state.log += "Cleaned snake pattern box\n"
        box.state.state.update({'snake': 'seen'})
    else:
        return box

    new_box.size = box.size
    new_box.state = box.state  # make sure you sync up the state properly
    return new_box
remove_snake_pattern
```

```text
<function remove_snake_pattern at 0x10b5ced30>
```

Now lets call this processor for our raw boxes

```python
processors = [remove_snake_pattern]
processed_box = []
for box in raw_boxes:
    print(f"for box {box.pattern}")
    for processor in processors:
        print(f"State id before Process {id(box.state)}")
        box = processor(box)
        print(f"State id after Process {id(box.state)}")
    processed_box.append(box)

print(state)
```

```text
for box flower
State id before Process 4486205840
State id after Process 4486205840
for box bird
State id before Process 4486205840
State id after Process 4486205840
for box snake
State id before Process 4486205840
State id after Process 4486205840
for box dust
State id before Process 4486205840
State id after Process 4486205840
ProcessState(box_type='generic', state={'snake': 'seen'}, box_count=4, log='Cleaned snake pattern box\n')
```

It was bugging me that i had to use an extra line to make sure that i attach or assign the `ProcessState` to all `Box` object explicitly. It felt redundant and required me to explicitly mention why it was done. In addition to that we need to make sure that the new document state object is not created and attached during the processing flow.


## Implementing Singleton {#implementing-singleton}

To cure my above itch and reduce that extra work of making sure that i attach the state when moving from box to box or making sure that only a single `ProcessState` object is created i thought of introducing `Singleton` pattern to my code-base.

To implement the [singleton]({{< relref "20240521120415-design_patterns.md#singleton" >}}) pattern i took the reference from this&nbsp;[^fn:2] stackoverflow thread

```python
class Singleton(type):
    """
    A metaclass that creates a Singleton base class when called.

    Read more here:
    https://stackoverflow.com/questions/6760685/what-is-the-best-way-of-implementing-singleton-in-python

    Also, Destroying a Singleton object in Python
    https://stackoverflow.com/questions/43619748/destroying-a-singleton-object-in-python
    https://stackoverflow.com/questions/63985051/how-to-reset-a-singleton-instance-in-python-in-this-case
    """

    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            instance = super(Singleton, cls).__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]

    def clear(cls):
        cls._instances = {}


@dataclass
class ProcessStateV2(metaclass=Singleton):
    box_type: str
    state: Dict = field(default_factory=dict)
    box_count: int = 0
    log: str = ""


state = ProcessStateV2(box_type="generic")
print(state)
print(id(state))
```

```text
ProcessStateV2(box_type='generic', state={}, box_count=0, log='')
4486506432
```

Based on this new `ProcessState` class definition we need to slightly modify our definition of the `Box` class to truly utilize the singleton design pattern

```python
@dataclass
class BoxV2:
    color: str = "white"
    size: Tuple[int, int] = (0, 0)
    pattern: str = "flower"
    state: ProcessStateV2 = ProcessStateV2(box_type="generic")

box = BoxV2()
print(box)
print(id(box.state))
```

```text
BoxV2(color='white', size=(0, 0), pattern='flower', state=ProcessStateV2(box_type='generic', state={}, box_count=0, log=''))
4486506432
```

Why Initialize the ProcessState on the Box Object Initialization Now?

-   It was done to utilize the singleton pattern. For now on, any `Box` object we create will only have a single `ProcesState` object to refer to.

<!--listend-->

```python
raw_boxes = [
    BoxV2(),
    BoxV2(color='pink', size=(1,1), pattern="bird"),
    BoxV2(color='green', size=(3, 3), pattern="snake"),
    BoxV2(color='purple', size=(2, 4), pattern="dust")
]

for box in raw_boxes:
    state.box_count += 1
    print(id(box.state))
print(boxes[0])
print(state)
```

```text
4486506432
4486506432
4486506432
4486506432
Box(color='white', size=(0, 0), pattern='flower', state=ProcessState(box_type='generic', state={}, box_count=4))
ProcessStateV2(box_type='generic', state={}, box_count=4, log='')
```

Lets also refactor the processor to utilize new definition.

```python
def remove_snake_pattern_v2(box: BoxV2) -> BoxV2:
    """
    A business logic processor that modifies and creates a new box

    Removes snake patterns and replaces with default box attribute
    """
    if box.pattern == "snake":
        new_box = BoxV2()
        new_box.state.log += "Cleaned snake pattern box\n"
        new_box.state.state.update({'snake': 'seen'})
    else:
        return box

    new_box.size = box.size
    return new_box
remove_snake_pattern_v2
```

```text
<function remove_snake_pattern_v2 at 0x10b6a6670>
```

In this refactored function we can see that we have removed the explicit assignment of \`state\` to the new `Box` object. Now a dev can focus on the business logic and won't have to remember to preserve the state or copy the state before modifying it. Here `ProcessState` flows through the boxes as a global variable.

Now lets call this refactor processor for our raw boxes

```python
processors = [remove_snake_pattern_v2]
processed_box = []
for box in raw_boxes:
    print(f"for box {box.pattern}")
    for processor in processors:
        print(f"State id before Process {id(box.state)}")
        box = processor(box)
        print(f"State id after Process {id(box.state)}")
    processed_box.append(box)

print(state)
```

```text
for box flower
State id before Process 4486506432
State id after Process 4486506432
for box bird
State id before Process 4486506432
State id after Process 4486506432
for box snake
State id before Process 4486506432
State id after Process 4486506432
for box dust
State id before Process 4486506432
State id after Process 4486506432
ProcessStateV2(box_type='generic', state={'snake': 'seen'}, box_count=4, log='Cleaned snake pattern box\n')
```

And Done. This is how applied [singleton]({{< relref "20240521120415-design_patterns.md#singleton" >}}) to refactor for my project.


## A pitfall to be aware of {#a-pitfall-to-be-aware-of}

Be aware that the Singleton pattern will restrict the `ProcessState` class to have only a single object. When such an object is used in an API request, we need to ensure that it is cleaned or deleted after the request; otherwise, the same object will be reused.

How did I handle it in my API?

We implemented a method that clears out the class instance(s) created, i.e., `ProcessStateV2.clear()`

```python
ProcessStateV2.clear()
state = ProcessStateV2(box_type="generic")
state.log += "new state created"
print(f"{id(state)} - {state}")
state2 = ProcessStateV2(box_type="new_generic")
print(f"{id(state2)} - {state2}")

ProcessStateV2.clear()

state3 = ProcessStateV2(box_type="cleaned")
print(f"{id(state3)} - {state3}")
ProcessStateV2.clear()
```

```text
4486216192 - ProcessStateV2(box_type='generic', state={}, box_count=0, log='new state created')
4486216192 - ProcessStateV2(box_type='generic', state={}, box_count=0, log='new state created')
4486262304 - ProcessStateV2(box_type='cleaned', state={}, box_count=0, log='')
```


## P.S {#p-dot-s}

-   Do go through&nbsp;[^fn:2] and [1.2](#org-target--r2)
-   There might be better way to handle this of-course but as of now i could only think of this solution

All this changes resulted in adding more lines of code then removing (refactoring i guess).

{{< figure src="/ox-hugo/2024-05-23_16-20-22_screenshot.png" >}}

[^fn:1]: ["Metaprogramming in Python using Metaclasses" - Adarsh Divakaran (PyCascades 2023)](https://www.youtube.com/watch?v=-js0K7Q878c)
[^fn:2]: [what-is-the-best-way-of-implementing-singleton-in-python](https://stackoverflow.com/questions/6760685/what-is-the-best-way-of-implementing-singleton-in-python)
