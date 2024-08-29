## Books
- [100 Go mistakes and How to avoid them](obsidian://open?vault=Tech%20Books&file=Golang%2F100%20Go%20Mistakes%20and%20how%20to%20avoid%20them)
- Learning Go - The idiomatic approach

### Effective GO

Reference: [https://go.dev/doc/effective_go](https://go.dev/doc/effective_go)


### [Learn GO Fast - Alex Mux](https://www.youtube.com/watch?v=8uiZC0l4Ajw)
- statically typed (every variable must have a type assigned or interpreted)
- strongly typed (can't mix types automatically)


### Packages
- Folder that contains collection of GO files
- Each file in a package must declare package name at the top
- Package name is the same as the last element of the import path
- `math/rand` package comprises files that begin with statement `package rand`
- `main` package should have a main function which will be the entrypoint of the code
#### Imports
```go
// Factored imports (good style)
import (
	"fmt",
	"math"
)

// normal imports
import "fmt"
import "math"

```

An Exported Name begins with an Uppercase letter. So while importing a package, we can only refer to the Exported Names

Ex. `math.Pi` is valid. But `math.pi` is not valid

### Functions
- Start with `func` keyword
- can take 0 or more arguments
- can return multiple results `return 1, 2`
- `Naked return` : Document the meaning of return values in function signature, and only write return statement in the end of the function
```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
```
- Outside a function, every statement begins with a keyword
### Variables
> Types come after variable name

- `x int`
- `func add(x int, y int) int`

For declaring multiple names, can write type at the end
- `x, y, z int`

##### var
- declares list of variables
- can be at package or function level
- `type` is last
- `var i, j, k int`
- variables can be initialized. if an initializer is present, the type can be omitted (will be inferred from the value)
- `var i = 1;`

##### Short Assignment `:=`
- Can be used inside a function to declare variables without using `var`
- `k := 3`

##### Factored Variable Declarations
```go
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)
```

> Variables declared without an explicit initial value are given their ZERO Value (false, 0, "")
> 
> Assignment between items of different type requires explicit conversions 

##### Constants - `const`

### Looping

GO only has a `for` loop
- parentheses are not required
- braces are required

A loop is made up of 3 statements
- init (optional) - runs before the first iteration
- condition - runs before each iteration
- post (optional) - runs at the end of each iteration

> Loop stops evaluating when the conditional boolean returns false
> Loop without a conditional runs forever

- variables defined inside loop are only available in the scope of loop

```go
// loop with all 3 conditions
for i := 0; i < 10; i++ {
	sum += i
}

// loop with only conditional
sum := 0
for ; sum < 1000; {  // can even drop semicolons here
	sum += 1
}
```

#### Range
- Used to iterate over a slice or map. Returns an index and value to iterate over for the length of the slice / map
- can skip either index or value by using `_` in its place
- if only index is required, can skip the second parameter
```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
for index, value := range pow {
	fmt.Printf("2**%d = %d\n", index, value)
}
for index := range pow {
	fmt.Printf("2**%d = %d\n", index, 2**index)
}
```


### Conditionals

#### if
- parentheses are not required
- braces are required
```go
if x < 0 {
	return false
}
```

- can write a short statement to execute before the condition. variables defined here are only in scope of `if` and corresponding `else` blocks

```go
	if v := math.Pow(x, n); v < lim {
		return v
	}
```

#### Switch
```go
os = 1
switch os {
	case "1": fmt.Println("1");
	default: fmt.Println("default")
}

switch time.Now().Weekday() {
    case time.Saturday, time.Sunday:
        fmt.Println("It's the weekend")
    default:
        fmt.Println("It's a weekday")
}

t := time.Now()
// alternate to if, same as 'switch true'
switch {
    case t.Hour() < 12:
        fmt.Println("It's before noon")
    default:
        fmt.Println("It's after noon")
}
```
- Like in `if` a short statement can be executed before `switch` starts execution, and the variable declared there is only available within the switch block.
- can combine multiple conditions in a single case
- It only runs the first block with matching `case` statement. `default` statement is not required
- The `case` need not be a constant

### `defer`

- defers the execution of a function until the surrounding function returns
- multiple defer calls are pushed onto a stack, and executes in LIFO order

```go
// print all numbers from 10 to 0
for i := 0; i <= 10; i++ {
	defer fmt.Println(i)
}

```

### Modules
- Collection of Packages

```go
go mod init <project-name>/<project-url>
```

Continue [here](https://go.dev/tour/moretypes/1)


### Other Data Types
#### Pointers

- A pointer holds the memory address of a value. Its declared using a `*` operator.
- Its zero (default) value is `nil`
- `&` operator generates a pointer to its operand
- `*` operator denotes the pointer's underlying value

```go
var p *int    // pointer of type int

var i int = 42;

p = &i;

fmt.Println(*p) // prints 42
```

#### Struct
- Collection of multiple fields
- Its fields are accessed using `.` operator
- A struct pointer can be made to access the struct. 
- It does not have to be de-referenced explicitly. (Can write ptr.field_name)
- Can skip all fields while allocating to set to default value
- Can pass values for a subset of field using `Name: Value` notation

```go
type Vertex struct {
	X int
	Y int
}

v := Vertex{1, 2}
fmt.Println(v.X) // prints 1

p := &v

fmt.Println((*p).X) // prints 1
fmt.Println(p.X) // also prints 1


var v1 Vertex = Vertex{1, 2}
var v2 Vertex = Vertex{}    // initialized to 0
var v3 Vertex = Vertex{X: 1}    // initialized to 0
```


#### Arrays

> Arrays can't be resized

- Can be defined as `[x]T` . This is an array of Type T with x entries
- Array can be initialized by providing initial values for the array

```go
var a [2]string
a[0] = "Hello"
a[1] = "World"

primes := [6]int{2, 3, 5, 7, 11, 13}  // 2,3,5,7,11,13
primes := [6]int{2, 3, 5, 7, 11}  // 2,3,5,7,11,0
```

#### Array Slice
- Dynamically-sized fixed view of an array
- Reference to an array. Modifying a slice will modify the actual array
- Can also define a new array using `slice` notation without specifying the size
- Has both `length` and `capacity`
- Its zero value is `nil`

```go
primes := [6]int{2, 3, 5, 7, 11, 13}
var s []int = primes[1:4]

prime[:] // includes all elements
prime[:6] // includes all elements from the beginning to index 5
prime[2:] // includes all elements from the index 2 to the end
s[0] = 15  // This will also change the primes[1] element to 15

q := []int{1,2,3,4,5,6}
```

- `length`: No of elements a slice contains
- `capacity`: No of elements in the underlying array
```go
s := []int{2,3,4,5,6}

s = s[:0]  // len: 0, capacity: 6
s = s[:4]  // len 4, capacity: 6
s = s[:8]  // Will give an error
len(s)   // prints length of the array
cap(s)  // prints capacity of array
```

`make`: built-in function to create a slice. It allocates a zeroed array and returns a slice that refers to that array

```go
make(type, length, [capacity]) [capacity or length]type
```

`append`: Built-in function to append new elements to a slice. It takes a slice (works with nil slice also), and adds all the elements passed after the first argument.
- If the underlying array does not have the capacity to add a new element, a new underlying array will be allocated `typically doubling the capacity`

```go
append(slice, ...T) []T
```



Continue [here](https://go.dev/tour/moretypes/18)
