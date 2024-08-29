
[Book PDF](https://acrobat.adobe.com/id/urn:aaid:sc:AP:77e1e52d-178d-41b0-8b0c-a5a35adb5f28)
 
### Class
- Implementation of an Object
### Abstract Class

> Cannot be instantiated

- Gives common interface with functionality for a concrete class

### Mixin Class
- Provides an optional interface to another class
### Object
- Data + Procedure that operates on data
- Instance of a **Class**

### Inheritance
- Child class derives functionality from parent class(es)
- Breaks encapsulation - because child class has to override methods defined in the parent class
```py
class Animal:
	def __init__(self, name):
		self.name = name
	def speak(self):
		pass

class Dog(Animal):
	def __init__(self, name):
		super.__init__(name)

	def speak(self):
		print("woof")
```
### Object Encapsulation
- Use simple objects to build complex objects
- Add reference of an object in other objects

```py
class Car:
	def __init__(self, name):
		self.name = name
	def drive(self):
		print("Driving ", self.name)

class Person:
	def __init__(self, person_name, car_name):
		self.name = person_name
		self.car = Car(car_name)
	def go_to_work(self):
		print(f"{self.name} going for work in ", self.car.name)
```

> A system should allow for change easily, to ensure flexibility
> A design pattern should be used if the flexibility it provides is greater than its inherent complexity


## Designing a Text Editor


### 1. Document Structure

- Need to maintain **physical structure** of the document i.e. arrangement of text and graphics into lines, columns, tables etc.
- Needs to generate and **present the document visually** on the screen
- Need to **map** the position of user cursor on the display monitor to an element inside the document structure
- Treat TEXT and GRAPHICS elements uniformly.

#### [Recursive Composition](https://refactoring.guru/design-patterns/composite)

- Don't distinguish between a single component and a complex diagram (group of components)
- Each component should know how to render itself
- Each component should trigger the rendering of its children
- Build increasingly complex elements out of simple ones

- `GLYPH` : Abstract class for all objects (type of components) that can appear in a document
- Each glyph type should know
	- How to draw themselves
	- What space they occupy
	- Their parent and children
	- If a point on display monitor is within its rendered area

### 2. Formatting

The formatting algorithm should be kept independent of the physical document structure so that we could add a new kind of Glpyh subclass without changing the formatting algorithm.

Similarly, adding a new formatting algorithm shouldn't require modifying existing glyphs

**KEY CONCEPTS**

- *Compositor Class*: Responsible for formatting GLYPHS based on a specific formatting algorithm
- *Composition*: Subclass of Glyph, contains the visible glyphs that constitute the document's content. It does not contain glyphs that represent the physical structure of the document (such as rows and columns). It gets an instance of "Compositor" subclass on creation.

When a composition object requires formatting, it calls its assigned subclass to perform the formatting operation.

The compositor, iterates through the composition's children (visible glyphs) and inserts additional glyphs (rows, columns) based on its line-breaking algorithm.

#### [Strategy](https://refactoring.guru/design-patterns/strategy)

- To encapsulate an algorithm in an object
- Each **STRATEGY** object contains different algorithm implementation


