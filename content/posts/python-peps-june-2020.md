+++
title = "Python super PEP week"
publishdate = "2020-06-24"
draft = false
+++

This week was really busy in the PEP side in Python. Three new PEPs were published touching internal and externals of the language. Here is a snapshot for y'all.

All PEPs are still under discussion but given the proponents, it is expected that they will progress well into the Python 3.10 timeline (which is also a PEP, PEP-619).


**PEP-620**:  Hide implementation details from the C API*

This one is under the hood for most of us but will enable more aggressive changes in the runtime. Many of us struggle with pure Python performance and sometimes even think that the core developers are kind of lazy on this aspect. Why other dynamically typed languages are so fast? (I am looking at you Javascript and Julia). One the reason is the badly exposed inner workings of the runtime. Any C extension can use internals and depend on it for years. Compatibility is a must, so Python core developers get stuck.

This PEP will allow internals to change without breaking compatibility. It is a work in progress, some of my patches in 3.9 were related to it.


**PEP-621**: Storing project metadata in pyproject.toml

This could be the final of `setup.py`. Standardized metadata and move to TOML based files (no YAML, no .py), TOML are very sane and future proof.


**PEP-622** Structural Pattern Matching

This is huge! If you have used Scala, Rust, and even Java in recent versions, pattern matching is a joy. It can transform very long if/elif/else blocks into readable code. This will probably change a little over the next year, but I expect this will be in 3.10 next year.

As the PEP says, this is more about object, dict and sequence destructuring than just a new way of using if/elif/else, so expect many new patterns to emerge from this new construct.

This work was possible after a new PEG-based parser landed in Python 3.9. The old parser was very limited and hard to work, according to Guido, the old parser was one of the first pieces he wrote in Python back in the 90s.

An example from the PEP, actual code from the standard library:

before:

```python
def is_tuple(node):
    if isinstance(node, Node) and node.children == [LParen(), RParen()]:
        return True
    return (isinstance(node, Node)
            and len(node.children) == 3
            and isinstance(node.children[0], Leaf)
            and isinstance(node.children[1], Node)
            and isinstance(node.children[2], Leaf)
            and node.children[0].value == "("
            and node.children[2].value == ")")
```

after:

```python
def is_tuple(node: Node) -> bool:
    match node:
        case Node(children=[LParen(), RParen()]):
            return True
        case Node(children=[Leaf(value="("), Node(), Leaf(value=")")]):
            return True
        case _:
            return False
```

- [PEP-619] https://www.python.org/dev/peps/pep-0619/
- [PEP-620] https://www.python.org/dev/peps/pep-0620/
- [PEP-621] https://www.python.org/dev/peps/pep-0621/
- [PEP-622] https://www.python.org/dev/peps/pep-0622/