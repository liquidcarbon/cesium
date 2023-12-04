# Project Cesium

How to design the all-powerful, hyperfine computational notebook?

Consider a simple Jupyter notebook with three cells:

```python
# cell 0 (markdown)
"Title"

# cell 1 (code)
import sys

# cell 2
print(sys.prefix)
```

What is the data model for that?  Let's write some sentences:
- each cell **contains** metadata and code
- cell metadata **describes** certain properties of the cell and **logically links** them to other cells (parent/child, usually an implicit relationship)
- cell code is a string of instructions that enable some of the following relationships:
  - **references** 0 to many global namespace objects
  - **defines** 0 to many global namespace objects
  - **outputs** a string (more on that later)

Specifically:
- cell 0 **is a logical parent** to cells 1 and 2
- cell 0 **references** nothing
- cell 0 **defines** nothing
- cell 0 **outputs** nothing
- cell 1 **is a logical child** of cell 0
- cell 1 **references** a global namespace object called `import` (a simplification)
- cell 1 **defines** a global object called `sys`, adding it to global namespace
- cell 1 **outputs** nothing
- cell 2 **is a logical child** of cell 0
- cell 2 **references** global objects `sys` and `print`
- cell 2 **defines** nothing
- cell 2 **outputs** the string representation of the `sys.prefix` attribute


## Code execution

Will the code work the same on different machines?  _Probably_ not.  
Will the code work the same at different times on the same machine?  _Probably_ yes.  

The most interesting property of a code cell is its executions (`Ex`).


## Pseudocode

```python
class Cell:
  id: ULID
  metadata: CellMetadata
    type: Enum(markdown|language1|language2|...)
    description: str
    parent: cell_ulid
    child: List[Cell]
  code: str
  references: List[object]
  definitions: List[object]
  executions: List[Ex]
    env: env_ulid  # should come out of package/environment managers (venvs, Conda, Poetry, Pixi)
    start: ULID
    end: ULID
    output: str
```

TODO: elaborate

## Name

The [Cesium Standard](https://en.wikipedia.org/wiki/Caesium_standard) is how we define time:
> *"The second is the duration of 9192631770 periods of the radiation corresponding to the transition between the two hyperfine levels of the ground state of the caesium 133 atom."* —Bureau international des poids et mesures

The Cesium Notebook Standard is how we may come to define computational notebooks.
