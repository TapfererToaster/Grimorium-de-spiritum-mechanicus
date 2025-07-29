[Learn to become a Go developer](https://roadmap.sh/golang)

>[!tip] Übungen aus LF5 in Go schreiben

# Go the Toolchain
## Package Management
`go mod` ("Go modules") is Go's built in package/dependency manager
## Testing
Go provides a system for testing that includes a package in the standard library, a command-line runner, code-coverage reporting and race-condition detection.
The naming convention for test files is that they end in `_test.go` and the function to test has to begin with `Test`. 
When running `go test`, the test function is executed
## Code Coverage
With the command `go test -cover` reports code coverage alongside the other details about the tests.
# Installing Go
Everything you need to know about getting and installing Go can be found [here](https://go.dev/doc/install).

# Hello Go
This application prints the message through a web server.
>[!important]
>To begin, start in a directory where you want your application to be.
>Next open a terminal and enter `go mod init hellogo` to initialize a new application.
>You will now see a file called `go.mod`, which contains module information and dependencies.
>After initializing your project create a `main.go` file in that directory.

```Go
// 1. main package is used for the application
package main

// 2. import needed packages
import (
	"fmt"
	"net/http"
)

// 3. Handler for an HTTP request
func hello(w http.ResponseWriter, r *http.Request) {
	fmt.Fprint(w, "Hello my name is Skynet")
}

// 4. main application execution
func main() {
	http.HandleFunc("/", hello)
	http.ListenAndServe("localhost:4000", nil)
}
```
## Running the application
You can run the application with the console command `go run file.go` 
or you can built and run the application with the console command
```Go
go build inigo.go
```
>[!note]
>Using `go build` without filenames, will build the current directory.
>Using a filename, set of filenames or wildcard pattern only builds the selection.

From here the build application needs to be executed. By convention, compiled binaries are created in the `bin` directory, but you can choose the destination with the `-o` flag
```Go
go build -o bon/inigo inigo.go
```

# Command-Line Flags
```Go
package main
import (
	"flag"
	"fmt"
)

var name = flag.String("name", "World", "A name to say hello to.")
var inspanish bool
func init(){
	flag.BoolVar(&inSpanish, "spanish", false, "Use Spanish language.")
	flag.BoolVar(&inSpanish,"s", false, "Use Spanish language.")
	flag.Parse()
}

func main() {
	if inSpanish {
		fmt.Printf("Hola %s!\n", *name)
	} else {
		fmt.Printf("Hello %s!\n", *name)
	}
}
```
This application is a variation of the standard "Hello World" program, but it also accepts flags. Here you can see two ways of defining a flag. 
1. In the first , a string variable is created from a flag, using `flag.Sting()`, which takes a flag name (name), default value (World!) and description as arguments.
>[!note]
>To access `name` you need a pointer `*name` 
2. The second way is to define a variable of the same type as the flag, which will be used to store the flag. Then use a `flag.TypeVar()` function that places the flags value into the variable. Finally you need to use `flag.Parse()` to set the flag value into the variable.
>[!note]
>Flag functions exist for each basic data type, which all follow a similar pattern, f.e. `flag.StringVar` or `flag.IntVar`.
>To learn more see the [flag package](https://pkg.go.dev/flag).
# Variables and Constants
## Declaration

To declare a variable you need the keyword `var`. 
You can declare multiple variables of the same datatype by writing them behind each other in one line, separating them with a `,`:
```Go
var firstname string
var firstname, lastname string
```
You can also declare variables as a "block" by using paranthesis:
```Go
var (
firstName, lastname string
age int
)
```
## Initialization
When you initialize a variable you do not need to specify it's datatype, as Go will infer the type during initialization:

```Go
firstName string = "John"
lastName string = "David"
age int = 32

firstname = "John"
lastName = "David"
age = 32
```
You can also initialize the variables in one line:
```Go
var (
firstName, lastName, age = "John", "David", 32
)
```
The most common way of declaring and initializing new variables is with the "colon equal sign" `:=`
```Go
func main() {
firstName, lastName := "John", "David"
age := 32
}
```
>[!warning]
>The colon equal sign can only be used with *new* variables, if you use it with a variable that has been declared already, the program will not compile.
>The colon equal sign can also only be used inside of functions, outside you must use the `var` keyword.

## Declare Constants

To declare constants, static values, you use the keyword `const`:
```Go
const HTTPStatusOk = 200
```
You can also declare constants as a "block" and do not need to specify the datataype:
```Go
StatusOK = 0
StatusConnectionReset = 1
StatusError = 2
```
>[!info]
>When constants are sequentitial you can use the keyword `iota`; read more [here]([Iota · golang/go Wiki · GitHub](https://github.com/golang/go/wiki/Iota))

>[!Warning]
>Go will throw an error if you declare variables/ constants and do not use them.

# Data types
## Integer
Besides the keyword `int` there are `int8`, `int16`, `int32` and `int64` which are `int` with the according size of bytes. 
The `int` size is usually tied to your OS, when using a 32 bit OS `int` has a size of 32 bit, it is the same for 64 bit OS. 
If you need a unsinged int you can use `uint`, `uint8`, `uint16`, `uint32` and `uint64`.
>[!warning]
>It is important to know that f.e. `int` is not the same as `int32`, even if it is on a 32 bit OS.
>Meaning you have to explicitly cast when it is needed and if you try to perform math with differnt `int` it will cause an error.

**Example: Use of various integer types:**
```Go
var integer8 int8 = 127
var integer16 int16 = 32767
var integer32 int32 = 2147483647
var integer64 int64 = 9223372036854775807
```
>[!info] Runes
>A `rune` is an alias for `int32` data type, used to represent Unicode characters:
>```
>rune := 'G'
>fmt.Println(rune)
>=> 72
>```

### Challenges
**1.)**
set a variable as `int` and use the value of int32 or int64 to confirm the natural size of you computer; if you use a value higher than 2.147.483.647 on a 32-bit system, you will get an error message
```Go
package main
import "fmt"

func main() {
	var integer int = 2147483647
	fmt.Println(integer)
}
```
**2.)**
Declare an variable as `uint` and initialize it with a negative value (like -10); when you try to run the porgram you should get an error message
```Go
package main
import "fmt"
func main() {
	var uinteger uint = -10
	fmt.Println(uinteger)
}
```
## Floating-point Numbers

In Go there are two types of floating point number, `float32` and `float64`, with the difference being the maximum size of bits they can hold
```Go
var varfloat32 float32 = 2147483647
var varfloat64 float64 = 9223372036854775807
fmt.Println(varfloat32, varfloat64)
```
By using the `math.MaxFloat32` and `math.MaxFloat64` constants you can find the limits of the two float types
```Go
package main
import (
	"fmt"
	"math"
)  

func main() {
fmt.Println(math.MaxFloat32, math.MaxFloat64)
}
```
Float is also used to store decimal numbers 
```Go
const e = 2.71828
const Avogadro = 6.02214129e23
const Planck = 6.62606957e-34
```
## Booleans
The boolean data type has only two values: `true` and `false` and use the keyword `bool` to declare them:
```Go
var varbool bool = true
```
>[!warning]
>In Go you can not implicitly convert a boolean type to `0` or `1`, you have to do it explicitly.
## Strings
You use the `string` keyword to declare strings.
```Go
var firstName string = "John"
lastName := "Doe"
var singleChar string = 'A'
singleChar2 := 'B'
```
>[!info]
>The single quotation marks `' '` are used to define single character values and runes
>

### Escape Characters
To use escape characrers you use the backslash `\` before the character.
The most common escape characters are:
- `\n` new line
- `\r` carriage returns
- `\t` tab
- `\'` single quotation marks
- `\"` double quotation marks
- `\\` backslah

```Go
fullName := "Hello, User\n my Name is \"User2\""
```
## Default values
When you declare a variable but do not initialize it with a value, it will be given a default value:
- `int`: 0
- `float`: +0.000000e+000
- `bool`: false
- `string`: empty value

```Go
var defaultint int
var defaultfloat32 float32
var defaultfloat64 float64
var defaultbool bool
var defaultstring string
fmt.Println(defaultint, defaultfloat32, defaultfloat64, defaultbool, defaultstring)
```
## Type Conversions
Implicit type conversions do not work in Go. 
To explicitly type convert you can use the built-in function
```Go
var integer16 int16 = 127
var integer32 int 32 = 32767
fmt.Println(int32(integer16) + integer32)
```
To convert string to int or vice-versa you can use the [strconv package]([strconv package - strconv - Go Packages](https://pkg.go.dev/strconv)) 
```Go
package main
import(
"fmt"
"strconv"
)

func main() {
i, _ := strconv.Atoi("-42") // string to int; Atoi returns 2 arguments 
s := strconv.Itoa(-42)  // int to string
fmt.Println(i, s)
}
```
>[!info]
>The underscore `_` means that we are not going to use the variable's value and we want to ignore it.

>[!info] Data type ranges
>You can see the range of each data type in the [Go source code]([- The Go Programming Language](https://go.dev/src/builtin/builtin.go))

# Functions
## Main function
All executable programs belong in this function as it is the starting point of the program.
To read command-line arguments you can use the [os package](https://pkg.go.dev/os) and the `os.Args` variable.
```Go
// This program reads two numbers from the command line and sums them up
package main
import (
	"fmt"
	"os"
	"strconv"
)

func main() {
	number1, _ := strconv.Atoi(os.Args[1])
	number2, _ := strconv.Atoi(os.Args[2])
	fmt.Println("Sum", number1+number2)
}
```
## Custom functions
The syntax for creatingfunctions is:
```Go
func name(parameter) return {
	content
}
```
The `func` keyword defines a function and assigns a name to it. After that you specify the number and type of parameters and the results that will be returned by the function 
```Go
package main

import (
"fmt"
"os"
"strconv"
)

func main() {
sum := sum(os.Args[1], os.Args[2])
fmt.Println("Sum:", sum)
}

func sum(number1 string, number2 string) int{
	int1, _ := strconv.Atoi(number1)
	int2, _ := strconv.Atoi(number2)
	return int1 + int2
}
```
You can also set the name of the return value as it were a variable
```Go
func sum(number1 string, number2 string) (result int) {
int1, _ := strconv.Atoi(number1)
int2, _ := strconv.Atoi(number2)
result = int1 + int2
return
}
```
### Return multiple values
Functions can return multiple values, you can define them similar to the parameter
```Go
func calc(number1 string, number2 string) (sum int, mul int) {
	int1, _ := strconv.Atoi(number1)
	int2, _ := strconv.Atoi(number2)
	sum = int1 + int2
	mul = int1 * int2
	return
}
```
>[!warning]
>Remember that you need two variables to store the return values of `strconv`
>

```Go
func main() {
	sum, mul := calc(os.Args[1], os.Args[2])
	fmt.Println(sum)
	fmt.Println(mul)
}
```
>[!tip] 
>Remember that you can use underscore `_` to store values that you do not need or plan to use

```Go
func main() {
	sum, _ := calc(os.Args[1], os.Args[2])
	fmt.Println(sum)
}
```
## Change function parameter values (pointers)
When you pass a variable to a function, a local copy will be made and the original variable will not be affected by any changes
```Go
package main
import "fmt"

func main() {
	name := "John"
	updateName(name)
	fmt.Println(name)
}

func updateName(name string) {
	name = "David"
}
``` 
If you want to make a change to the original variable you need to use a pointer

>[!info] Pointers
>A pointer is a variable that contains the memory address of another variables

In Go there are two operators for pointers:
- `&`: takes the address of the object
- `*`: it gives you access to the object at the stored address in the pointer

```Go
package main

import "fmt"

func main() {
	firstname := "John"
	updateName(&firstname)
	fmt.Println(firstname)
}

func updateName(firstname *string){
*firstname = "David"
}
```
# Packages
Packages are like libraries or modules, as you can package your code and reuse it somewhere else. The source code can be distributed in more than one `.go` file.
The naming convention in Go is that a package uses the last part of it's import path as it's name.
```Go
import "math/cmplx"
cmplx.Inf()
```
## Main package
Usually the default package is the `main` package. If a program is part of this package, Go generates a binary file, a stand alone executable. 
If a program is not part of the `main` package, Go will generate an archive file (`.a` file).

## Import Packages
Packages are imported by their name, which can be a module or a URL.
```Go
package main
import "fmt"
func main() {
	fmt.Println("Hello World")
}
```

You can also group multiple imports in parentheses.
```Go
import(
	"fmt"
	"net/http"

	"golang.org/x/html"
)
```

>[!info] Conventions
>- Packages use the last part of it's import path as it's name
>- Package imports should be in alphabetical order and non-standard libraries come after standard library.


## Create a Package
1. Create a new directory called "calculator" in the `$GOPATH/src` directory
2. Create a file called `sum.go`  in the `calculator` directory
3. Initialize the file with the name of the package (`package calculator`) 
4. Write the variables and functions for your package

>[!info] Public and Private
>Go does not have the `public` or `private` keywords to indicate if a variable or function can be called form outside the package.
>Instead you to set a variable or function to public or private you:
>- start the name with lowercase letter to set it as private
>- start the name with uppercase letter to set it as public

```Go
package calculator

var logMessage = "[LOG]"

// Version number; can be called from outside
var Version = "1.0"

func internalSum(nimber int) int {
	return number - 1
}

// function can be called from outside
func Sum(number1, number2 int) int{
	return number1 + number2
}
```
## Create a Module
Modules typically contain packages that offer related functionality, it also specifies the context that Go needs to run the code you have grouped together.

>[!note]
>Modules makes working with dependencies easier and the program's source code does not need to exist in the `$GOPATH/src` directory.

To create a module for the calculator package, run the following command in the root directory (`$GOPATH/src/calculator`):
```
go mod init github.com/myuser/calculator
```
This sets the name of the module to `github.com/myuser/calculator`, which you will use to reference it in your projects. The command also creates a new file called `go.mod`.

## Reference a local package (a module)
```Go
package main

import (
	"fmt"
	"github.com/myuser/calculator"
)

func main() {
	total := calculator.Sum(3.5)
	fmt.Println(total)
	fmt.Println("Version", calculator.Version)
}
```
To run the program you have to tun the following command in your project directory ($GOPATH/src/myproject)
```
go mod init myproject
```
This creates a new `go.mod` file; open it an include the reference to the calculator package
```
module myproject

go 1.14
require github.com/myuser/calculator v0.0.0
replace github.com/myuser/calculator => ../calculator
```
Because `myproject` and `claculator` are both in `$GOPATH/src` the location is `../`

Run the program with:
```
go run main.go
```
## Reference a external package
```Go
package main

import (
	"fmt"
	"github.com/myuser/calculator"
	"rsc.io/quote"
)

func main(){
	total := calculator.Sum(3.5)
	fmt.Println(total)
	fmt.Println("Version", calculator.Version)
	fmt.Println(quote.Hello())
}
```
If you use Visual Studio Code, the `go.mod` file will update when you save the file
```
module myproject 
go 1.14 
require ( 
	github.com/myuser/calculator v0.0.0 
	rsc.io/quote v1.5.2 
) 
replace github.com/myuser/calculator => ../calculator
```
When you need to upgrade you code's dependencies, you will need to change the version number here.


# Control Flow
## If/else Statements
In Go you do not need parenthesis in conditions; the `else` clause is optional; but Go does not offer support for ternary if statements; you need to write the full `if` statement.
```Go
package main

import "fmt"

func main() {
	x := 27
	if x%2 == 0 {
		fmt.Println(x, "is even")
	}
}
```

### Compound if statements
You can nest statements by using the `else if` statement.
```Go
package main

import "fmt"

func giveanumber() int {
	return -1
}

func main() {
	if num := giveanumber(); num < 0 {
		fmt.Println(num, "is negative")
	} else if num < 10 {
		fnt.Println(num, "has only one digit")
	} else {
		fmt.Println(num, "has multiple digits")
	}
}
```
## Switch Statement
To avoid chaining many `if` statements together you can use a `switch` statement.
```Go
switch i {
	case 0:
		fmt.Println("zero")
	case 1:
		fmt.Println("one")
	case 2:
		fmt.Println("two")
	default:
		fmt.Println("default")
}
```
### Use Multiple Expressions
If you want that one `case` statement includes multiple expression, separate them with a comma `,`

```Go
package main

import "fmt"

func location (city string) (string, string) { 
	var region string
	var continent string
	switch city {
		case "Dehli", "Hyderabad", "Mumbai", "Chennai", "Kochi":
			region, continent = "India", "Asia"
		case "München", "Berlin", "Hamburg", "Stuttgart":
			region, continent = "Germany", "Europe"
		case "Irvine", "Los Angeles", "San Diego":
			region, continent = "California", "USA"
		default:
			region continent = "unknown", "unknown"
	}
	return region, continent
}

func main() {
	region, continent := location("Irvine")
	fmt.Println("John works in %s, %s\n", region, continent)
}
```
### Invoke a Function
A `switch` function can also invoke a function and use the return value as `case` statements
```Go
package main

import "fmt"

func main() {
	switch time.Now().Weekday().String() {
	case "Monday", "Tuesday", "Wednesday", "Thursday", "Friday":
		fmt.Println("Time to Work")
	default:
		fmt.Println("It's the weekend. Time to rest")
	}
	fmt.Println(time.Now().Weekday().String())
}
```
It is also possible to call a function from a `case` statement
```Go
package main

import (
	"fmt"
	"ragexp"
)
func main() {

	var email = regexp.MustCompile(`^[^@]+@[^@.]+\.[^@.]+`) 
	var phone = regexp.MustCompile(`^[(]?[0-9][0-9][0-9][). \-]*[0-9][0-9][0-9][.\-]?[0-9][0-9][0-9][0-9]`)

	var contact := "foo@bar.com"

	switch {
		case email.MatchString(contact):
			fmt.Println(contact, "is an email")
		case phone.MatchString(contact):
			fmt.Println(contact, "is a phone number")
		default:
			fmt.Println("not recognized")
	}
}
```
### Use a Condition
You can omit a condition in a `switch` statement like you can do in a `if` statement
```Go
package main

import(
	"fmt"
	"math/rand"
	"time"
)

func main() {
	rand.Seed(time.Now().Unix())
	r := rand.Float64()
	switch {
		case r > 0.1:
			fmt.Println("Common case")
		default:
			fmt.Println("Rare case")
	}
}
```
### Fallthrough
By default when one case is executed, the program exits the `switch` statement. 
You have to use the `fallthrough` keyword, if the programm should pass to the next `case` statement.
```Go
package main

import "fmt"

func main() { 
	switch num := 15 {
		case num < 50:
			fmt.Println("%d is smaller than 50\n", num)
			fallthough
		case num > 100:
			fmt.Println("%d is greater than 100\n", num)
			fallthrough
		case num < 200:
			fmt.Println("%d is smaller than 200", num)
	}
}
```

>[!warning] 
>When the program falls to the next `case` statement the condition will not checked again.

## Loops
To use a loop in Go you use the `for` keyword and seperate the three components with a semicolon `;`:
- pre-statement*: executes before the first iteration; (optional)
- *condition*: evaluated before every iteration; loop stops when this condition is `false`
- *post-statement*: executed at the end of every iteration; (optional) 

>[!info]
>In Go there are no `while` loops, but you can use `for` loops instead, as the initial statement and poststatement are optional.

```Go
func main() {
	sum := 0
	for i := 1; i <= 100, i++ {
		sum += 1
	}
	fmt.Println("sum of 1 ... 100 is", sum )
}
```
### Empty Prestatments and Poststatments

To use a `for` loop like a `while` loop you leave the pre- and poststatment out:
```Go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	var num int64
	rand.Seed(time.Now().Unix())
	for num != 5 {
		num = rand.Int63n(15)
		fmt.Println(num)
	}
}
```
### Infinite Loops and Break Statements
In the case of an infinite loop you do not write a condition, instead the logic to exit the loop is inside of it with the use of the `break` statement.
```Go
package main

import (
	"fmt"
	"math"
	"time"
)

func main() {
	var num int32
	sec := time.Now().Unix()
	rand.Seed(sec)

	for {
		fmt.Println("Writing inside the loop")
		if num = rand.Int31n(10); num == 5
		fmt.Println("finish") 
	} 
	fmt.Println(num)
}
```
### Continue Statements
You can use the `continue` Statement to skip the the current iteration of a loop
```Go
package main

import "fmt"

func main() {
	sum := 0
	for num := 1, num < 100, num++ {
		if num % 5 == 0 {
			continue
		}
		sum += num
	}
	fmt.Println("The sum of 1 to 100, excluding numbers dividable by 5, is: ", sum )
}
```
## Defer, Panic and Recover Functions

### Defer
The `defer` keyword signals that a function is postponed until the function that contains that statement is finished. The defer statements will be added to a stack and will be executed in reverse order (first in, last out):
```Go
package main

import "fmt"

func main() {
	for i := 1; i<4;i++ {
		defer fmt.Println("defered ", -i)
		fmt.Println("regular ", i)
	}
}
```
This can be used when you want to make sure that certain tasks are finished, like closing a file:
```Go
package main

import (
	"io"
	"os"
	"fmt"
)

func main() {
	newfile, error := os.Create("learnGo.txt")
	if error != nil: {
		fmt.Println("Error: Could not create this file")
		return
	}	
	defer newfile.Close()

	if _, error = io.WriteString(newfile, "Learning Go!"); error != nil {
		fmt.Println("Error: Could not write to file.")
		return
	}
	newfile.Sync()
}
```
### Panic
When runtime errors occur a Go program panics. 
You can also use the `panic()` function to stop the normal flow of control. Any deferred function calls run normally, after all functions on the stack have returned the program crashes with a log message. You can add any value to the `panic` function, but usually you add an error message why you are panicking.
```Go
package main

import "fmt"

func highlow(high int, low int) {
	if high < low{
		fmt.Println("Panic!")
		panic("highlow() low greater than high")
	}
	defer fmt.Println("Deferred: highlow(", high, ",", low, ")")
	fmt.Println("Call: highlow(", high, ",", low, ")")

	highlow(high, low + 1)
}

func main() {
	highlow(2, 0)
	fmt.Println("Program finished successfully!")
}
```
### Recover
When you want to avoid a program crash, report the error internally or clean the program before it crashes you can use the `recover()` function. You call `recover()` in  a function where you are also calling `defer()`. When called it returns `nil` and has no other effect in normal running.  
```Go
func main() {
	defer func() {
		handler := recover()
		if handler != nil {
			fmt.Println("main(): recover", handler)
		}
	}()
highlow(2, 0)
fmt.Println("Program finished successfully")
}
```
## Exercises
### Write a FizzBuzz program
This program should print the numbers 1 through 100, but:
- print `Fizz` if the number is divisible by 3
- print `Buzz` if the number is divisible by 5
- Print `FizzBuzz` if the number is divisible by both 3 and 5
- print the number if none

```Go
package main

import "fmt"

func main() {

	for i := 1; i <= 100; i++ {

		switch {
		case i%3 == 0:
			fmt.Println("Fizz")
		case i%5 == 0:
			fmt.Println("Buxx")
		case i%15 == 0:
			fmt.Println("FizzBuxx)
		default:
			fmt.Println(i)
		}
	}

}
```
### Find the Primes
Write a program to find all primes less than 20.

```Go
func findprimes(number int) bool {
    for i := 2; i < number; i++ {
        if number%i == 0 {
            return false
        }
    }
    if number > 1{
        return true
    }
    else {
        return false
    }
}
  
func main() {
    fmt.Println("Prime numbers less than 20:")
    for number := 1; number < 20; number++ {
        if findprimes(number) == true {
           fmt.Printf("%v ", number)
        }
    }
```
### Ask a number, panic if negative
```Go
func main() {
 fmt.Println("Enter a number")
 
    val := 0
    fmt.Scanf("%d", &val)
    switch {
    case val > 0:
        fmt.Println("You entered:", val)
    case val == 0:
        fmt.Println("0 is neither negative nor positive")
    case val < 0:
        panic("Number is negative")
    }
}
```
# Datenstrukturen
## Arrays
### Declare Arrays
You can declare arrays by using `[]` and writing the number of elements inside the brackets
```Go
var a [3]int
var b [5]string
```
By default the array will be initialized with the default data type (int: 0, string: empty value).

### Initialize arrays
To initialize arrays with other values you can:
```Go
var a [3]int
a[0] = 1
a[1] = 2
a[2] = 3

var b [4]string{"London", "Berlin", "Paris", "Rom"}
```
### Ellipsis in arrays
When you do not now how many positions in the array you will need, you can use ellipsis `[...]`:
```Go
q := [...]int{1, 2, 3}
```
### Multidimensional Arrays
To declare and initialize a two-dimensional array:
```Go
var twoA [3][5]int
```
```Go
package main

import "fmt"

func main() {
	var twoA [3][5]int
	for i := 0; i < 3; j++ {
		for j := 0; j < 5; j++ {
			twoA[i][j] = (i+1) * (j+1)
		}
		fmt.Println("Row", i, twoA[i])
	}
	fmt.Println("\nAll at once:", twoA)
}
```
You can use the same idea to create multidimensional arrays.
## Slices
Arrays are the foundation for slices and maps, but there are some differences.
Slices, unlike arrays, are dynamic and not fixed.

A slice is a data structure on top of an array or another slice, this originating slice or array is called the underlying array.

A slice has three components:
- pointer to the first reachable element of the underlying array (not necessarily the first element a[0])
- Length of the slice
- Capacity of the slice: number of elements between the start of the slice and the end of the underlying array.

### Declare and Initialize a Slice
You declare a slice the same way you declare an array.
```Go
months := []string{"January","Feburary","March", //...}
```
```Go
package main

import "fmt"

func main() {
	months := []string{"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"}
	fmt.Println(months)
	fmt.Println("Length:", len(months))
	fmt.Println("Capacity:", cap(months))
}
```
### Slice Items
Go supports the slice operator `s[i:p]` 
- `s` represents the array
- `i` represents the pointer to the first element of the underlying array (not necessarily the first element array[0])
- `p` represents the position of the last element in underlying array to use when creating the new slice
```Go
package main

import "fmt"

func main() {
	months := []string{"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"}
	quarter1 := months[0:3]
	quarter2 := months[3:6]
	quarter3 := months[6:9]
	quarter4 := months[9:12]
	fmt.Println(quarter1, len(quarter1), cap(quarter1))
	fmt.Println(quarter2, len(quarter2), cap(quarter2))
	fmt.Println(quarter3, len(quarter3), cap(quarter3))
	fmt.Println(quarter4, len(quarter4), cap(quarter4))
}
```
### Append Items
To append an item to a slice you can use the function `append(slice,element)`, which returns a new slice.
When the the number of items in the array reach the capacity limit, Go creates a new array and doubles the capacity.
```Go
package main

import "fmt"

func main() {
	var numbers []int
	for i := 0; i < 10; i++ {
		numbers = append(numbers, i)
		fmt. Printf("%d\tcap=%d\t%v\n", i, cap(numbers), numbers)
	}
}
```
### Remove Items
Go does not have a built-in function to remove elements from a slice, but we can use the slice operator `s[i:p]` to create a new slice without the elements we want to remove.
```Go
package main

import "fmt"

func main() {
	letters := []string{"A", "B", "C", "D", "E"}
	remove := 2

	if remove < len(letters) {
		fmt.Println("Before", letters, "Remove ", letters[remove])

		letters = append(letters[:remove], letters[remove+1:]...)
		fmt.Println("After", letters)
	}
}
```
You could also create a new copy of the slice without the element.
### Create Copies of Slices
To create a copy of a slice you can use the function `copy(dst, src []Type)`. Copies are used when you want to change a slice but not the underlying array, which would affect everyone pointing to that array. 
```Go
package main 

import "fmt"

func main() {
	letters := []string{"A", "B", "C", "D", "E"}
	fmt.Println("Before", letters)

	slice1 := letters[0:2]

	slice2 := make([]string, 3)
	copy(slice2, letters[1:4])

	slice[1] = "Z"
	fmt.Println("After", letters)
	fmt.Println("Slice2", slice2)
}
```
## Maps

A map in Go is basically a hash table, which is a collection of key and value pairs. The keys and values each must have the same data type, for example keys could be numbers and the values could be strings. 
To access a item you reference its key.

### Declare and Initialize a Map
 To declare a map you use the `map[key]value` keyword and define the key and value type.
```Go
package main

import "fmt"

func main() {
	studentAge := map[string]int{
		"john": 32,
		"bob": 31,
	}
	fmt.Println(studentsAge)
}
```
To create an empty map you can use the `make()` function.
```Go
studentsAge := make(map[string]int)
```
### Add Items
To add items to a map you define a key and a value, if the pair does not exist it will be added to the map.
```Go
package main 

import "fmt"

func main(){
	studentAge := make(map[string]int)
	studentAge["john"] = 32
	studentAge["bob"] = 31
	fmt.Println(studensAge)
}
```
>[!warning] 
>This does only work with a map that has been initialized. 
>To avoid problems always use `make()` to create empty maps.
>
### Access Items
To access an item in a map use `m[key]`.
```Go
package main

import "fmt"

func main() {
	studentsAge := make(map[string]int)
	studentsAge["john"] = 32
	studentsAge["bob"] = 31
	fmt.Println("Bob's age is", studentsAge["bob"])
}
```
>[!note]
>When you try to access an element that does not exist, instead of an error you will get the default value back. 
>To check if an item exists you can use:
>```Go
>age, exist := studentsAge["christy"]
>if exist {
>	fmt.Println("Christy's age is", age)
>}
>else {
>	fmt.Println("Christy's age could not be found")
>}
>```

### Remove Items
You can use the `delete()` function to remove items.
```Go
package main

import "fmt"

func main() {
	studentsAge := make(map[string]int)
	studentsAge["John"] = 32
	studentsAge["Bob"] = 31
	delete(studentsAge, "John")
	fmt.Println(studentsAge)
}
```

### Loop in a Map
To loop through a map you can use the range-based loop
```Go
package main 

import "fmt"

func main() {
	studentsAge := make(map[string]int)
	studentsAge["john"] = 32
	studentsAge["Bob"] = 31
	for name, age := range studentsAge {
		fmt.Printf("%s\t%d\n", name, age)
	}
}
```
## Structs
### Declare and Initialize a Struct
To declare a struct, you use the `struct` keyword, along with the list of fields and their types
```Go
type Employee struct {
	ID           int
	FirstName    string
	LastName     string
	Address      string
}
```
You can then declare a variable with the new type:
```Go
var john Employee
```
If you want to declare and initialize a variable at the same time:
```Go
employee := Employee{1001, "John", "Doe", "Doe's Street"}
employee := Employee{LastName: "Doe", FirstName: "John"}
```
To access individual fields of a struct, you use the `.` dot notation:
```Go 
employee.ID = 1001
fmt.Println(employee.FirstName)
```
You can use the `&` operator to yield a pointer to the struct:
```Go
package main

import "fmt"

type Employee struct{
	ID           int
	FirstName    string
	LastName     string
	Address      string
}

func main() {
	employee := Employee{LastName: "Doe", FirstName: "John"}
	fmt.Println(employee)
	employeeCopy := &employeeCopy
	employeeCopy.FirstName = "David"
	fmt.Println(employee)
}
```
### Struct embedding
Structs can be embedded within other structs, which can be useful when you want to reduce repetition and reuse a common struct. 
An example is that you want to have a data type for an employee and another one for a contractor, which could be resolved by having a `Person` struct.
```Go
type Person struct {
	ID           int
	FirstName    string
	LastName     string
	Address      string
}
```
You can then declare another type that embed a `Person` type, to embed a struct you create a new field
```Go
type Employee struct {
	Information Person
	ManagerID int
}
```
To reference a field from the `Person` struct, you will need to include the `Information` field from an employee variable
```Go
var employee Employee
employee.Information.FirstName = "John"
```
You could alternatively include a new field with the same name of the struct you are embedding
```Go
type Employee struct{
	Person
	ManagerID int
}
```
Example
```Go
package main

import "fmt"

type Person struct {
	ID           int
	FirstName    string
	LastName     string
	Address      string
}

type Employee struct{
	Person
	ManagerID int
}

type Contractor struct{
	Person
	CompanyID int
}

func main() {
	employee := Employee{
		Person: Person{
			FirstName: "John",
		},
	}
	employee.LastName = "Doe"
	fmt.Println(employee.FirstName)
}
```
## Exercises

**1.)**
Write a program which returns the Fibonacci sequence based on user input
```Go
package main

import "fmt"

func fibonacci(number int) []int {
    sequence := []int{1, 1}
    for i := 2; i < number; i++ {
        fibonaccinumber := sequence[i-1] + sequence[i-2]
        sequence = append(sequence, fibonaccinumber)
    }
    return sequence
}

func main() {
    fibonaccisequence := fibonacci(8)
    fmt.Println(fibonaccisequence)
}
```

**2.)**
Write a program that translates a Roman numeral (like MCLX) to a decimal number (like 1,160)
Solution from Course:

```Go
package main

import (
	"fmt"
)

func romanToArabic(numeral string) int {
	romanMap := map[rune]int{
		'M': 1000,
		'D': 500,
		'C': 100,
		'L': 50,
		'X': 10,
		'V': 5,
		'I': 1,
	}

	arabicVals := make([]int, len(numeral)+1)

	for index, digit := range numeral {
		if val, present := romanMap[digit]; present {
			arabicVals[index] = val
		} else {
			fmt.Printf("Error: The roman numeral %s has a bad digit: %c\n", numeral, digit)
			return 0
		}
	}

	total := 0

	for index := 0; index < len(numeral); index++ {
		if arabicVals[index] < arabicVals[index+1] {
			arabicVals[index] = -arabicVals[index]
		}
		total += arabicVals[index]
	}

	return total
}

func main() {
	fmt.Println("MCLX is", romanToArabic("MCLX"))
	fmt.Println("MCMXCIX is ", romanToArabic("MCMXCIX"))
	fmt.Println("MCMZ is", romanToArabic("MCMZ"))
}
```
# Error Handling 
Error handling in Go is a control-flow with `if` and `return` statements. 
```Go
employee, err := getInformation(1000)
if err != nil {
	// something is wrong
}
```
It is necessary that you pay attention to how you report and handle errors.
```Go
package main

import (
	"fmt"
	"os"
)

type Employee struct {
	ID           int
	FirstName    string
	LastName     string
	Address      string
}

func main() {
	employee, err := getInformation(1001)
	if err != nil {
		// something is wrong
	}
	else {
		fmt.Print(employee)
	}
}

func getInformation(id int) (*Employee, error) {
	employee, err := apiCallEmployee(1000)
	return employee, err
}

func apiCallEmployee(id int) (*Employee, error) {
	employee := Employee{LastName: "Doe", FirstName: "John"}
	return &employee, nil
}
```
## Error Handling Strategies
When a function returns an error, it is usually the last return value and it is the responsibility of the caller to check if an error occurred and handle it.
A common strategy is to propagate the error in a subroutine.
```Go 
func getInformation(id int) (*Employee, error) {
	employee, err := apiCallEmployee(1000)
	if err != nil {
		return nil, err // return the error to the caller
	}
	return employee, nil
}
```
You can also use the `fmt.Errorf()` function to include more information before propagating the error.
```Go
func getInformation(id int) (*Employee, error) {
	employee, err := apiCallEmployee(1000)
	if err != nil {
		return nil, fmt.Errorf("Got an error: %v", err)
	}
	return employee, nil
}
```

You could also retry when errors are transient, f.e retry a function three times and wait two seconds.
```Go
func getInformation(id int) (*Employee, error) {
	for tries := 0; tries < 3; tries++ {
		employee, err := apiEmployee(1000)
		if err == nil {
			return employee, nil
		}

		fmt.Println("Server is not responding, retrying ...")
		time.Sleep(time.Second * 2)
	}
return nil, fmt.Errorf("Error Error Error")
}
```

>[!note] 
>You could also log the errors and hide the details from the enduser

## Create reusable errors
Reusable errors can be used to maintain order when the number of errors increases or you can create a library for common messages. 
To create new errors and reuse them you can use the `errors.New()` function.
```Go
var ErrNotFound = errors.New("Employee not found")

func getInformation(id int) (*Employee, error) {
	if id != 1001 {
		return nil, ErrNotFound
	}
	
	employee := Employee{LastName: "Doe", FirstName "John"}
	return &employee, nil
}
```
With the `errors.Is()` function you can compare the type of error that occured:
```Go
employee, err := getInformation(1000)
if errors.Is(err, ErrNotFound) {
	fmt.Printf("Not found: %v\n", err)
}
else {
	fmt.Print(employee)
}
```
## Best Practices
- Always check for errors and handle them properly to avoid exposing unnecessary information to end users.
- Include a prefix error message which tells you in what package or function the error occurred
- Create reusable error variables 
- Panic if there is nothing else you can do. for example a dependency is not ready
- Log errors with as many details as possible

# Logging
## The log package
For simple logging you can use the `log` package, but for more complex logging configuration you can do it by using a logging framework.
```Go
import "log"

func main() {
	log.Print("Hey, I am a log!.")
}
```
>[!note]
>By default the `log.Print()` function includes the date and time as the log message's prefix.

With the `log.Fatal()` function you can log errors and end the program as if you would use `os.Exit(1)`.
```Go
package main

import (
	"fmt"
	"log"
)

func main() {
	log.Fatal("Error")
	fmt.Print("This will not be executed")
}
```
You can also use the `log.Panic()` function, which calls the `panic()` function.
```Go
package main

import (
	"fmt"
	"log"
)

func main() {
	log.Panic("Error")
	fmt.Print("This will not be executed")
}
```
Another essential function is `log.SetPrefix()`, to add a prefix to your program's log messages.
```Go
package main

import "log"

func main() {
	log.SetPrefix("main(): ")
	log.Print("I am a log")
	log.Fatal("Error")
}
```
>[!note]
>For other log functions see [log package - log - Go Packages](https://pkg.go.dev/log)

## Logging to a File
You can send logs to files to process them later or hide log information from the end users or to have logs from multiple applications in a centralized single location.
```Go
package main

import(
	"log"
	"os"
)

func main() {
	file, err := os.OpenFile("info.log", os.O_CREATE|os.O_APPEND|os.O_WRONGLY, 0644)
	if err != nil {
		log.Fatal(err)
	}
	defer file.Close()

	log.SetOutput(file)
	log.Print("Hey, I am a log!")
}
```
>[!tip]
>When you run this code there is no output on the console, but in your directory should be a new file called `info.log`.
>Note that you need to create or open a file and configure the `log` package to send the output to a file.

## Logging Framework (Zerolog)
>[!info]
>A few logging frameworks are [Logrus](https://github.com/sirupsen/logrus), [Zerolog](https://github.com/rs/zerolog), [Zap](https://github.com/uber-go/zap), [Apex](https://github.com/apex/log)

We first have to install the package by entering the command in the console
```Go 
go get -u github.com/rs/zerolog/log
```
```Go
package main

import (
	"github.com/rs/zerolog" 
	"github.com/rs/zerolog/log
)

func main() {
	zerolog.TimeFieldFormat = zerolog.TimeFormatUnix
	log.Print("I am a log")
}
```
>[!note]
>The output will change to JSON, a format used for logs when running searches in a centralized location.

You can include context data into the code.
```Go
package main

import (
	"github.com/rs/zerolog" 
	"github.com/rs/zerolog/log"
)

func main() {
	zerolog.TimeFieldFormat = zerolog.TimeFormatUnix

	log.Debug().
		Int("EmployeeID", 1001).
		Msg("Getting employee information")

	log.Debug().
		Str("Name", "John").
		Send()
}
```
# OOP
## Methods
### Declare Methods
The syntax to declare a method is
```Go
func (variable type) MethodName(parameters ...) {
	// method functionality
}
```
Before you can declare a method, you have to create a struct.

>[!example]
>You want to make a geometry package and, as part of it, you decide to create a triangle struct called triangle and a method to calculate the perimeter of that triangle.

```Go
type trianlge struct {
	size int
}

func (t triangle) perimeter() int {
	return t.size * 3
}

func main() {
	t := triangle{3}
	fmt.Println("Perimeter:", t.perimeter())
}
```
>[!warning]
>The `perimeter()` function will not work if you call it as other functions, because it needs a receiver. 
>The only way to call that method is to declare a struct, which gives you access to the method.

```Go
package main

import "fmt"

type triangle struct {
	size int
}

type square struct {
	size int
}

func (t triangle) perimeter() int {
	return t.size * 3
}

func (s square) perimeter() int {
	return s.size * 4
}

func main() {
	t := triangle{3}
	s := square{4}
	fmt.Println("Perimeter (triangle):", t.perimeter())
	fmt.Println("Perimeter (square):", s.perimeter())
}
```
### Pointers in Methods

Pointers are used when you want to update a variable or the argument to a method is too large so you want to avoid copying it. They are also used when you update the receiver variable in a method.
>[!example]
>You want to create a method to double the triangle size, you need to use a pointer in the receiver variable

```Go
func (t *triangle) doubleSize() {
	t.size *= 2
}

func main() {
	t := triangle{3}
	t.doubleSize()
	fmt.Println("Size:", t.size)
	fmt.Println("Perimeter:", t.perimeter())
}
```
>[!tip]
>When you only want to access the receiver's information you do not need a pointer in the receiver variable.
>Go convention dictates that if any method of a struct has a pointer receiver, all methods of that struct must have a pointer receiver, even if a method of the struct does not need it.
>
### Declare Methods for other Types
You can not define a struct from a type that belongs to another package, therefor you can not create a method on a basic type, like string.
You can however use a hack to create custom functions for basic types and use it as if it were the basic function.

>[!example]
>You want to create a method that transforms a string from lowercase to uppercase.

```Go
package main

import (
	"fmt"
	"strings"
)

type upperstring string

func (s upperstring) Upper() string {
	return strings.ToUpper(string(s))
}

func main() {
	s := upperstring("Learn learn learn")
	fmt.Println(s)
	fmt.Println(s.Upper())
}
```
### Embed Methods
Just like properties you can use methods in one struct and embed the same method in another struct.
>[!example]
>You want to create a new triangle struct with logic to include a color, but you want to continue using the triangle struct you declared before.

```Go
type coloredTriangle struct {
	triangle
	color string
}
```
You could initialize the `coloredTriangle` struct and call the `perimeter()` method form the `triangle` struct and access fields
```Go
func main() {
	t := coloredTriangle{triangle{3}, "blue"}
	fmt.Println("Size:", t.size)
	fmt.Println("Perimeter", t.perimeter())
}
```
```Go
func (t coloredTriangle) perimeter() int {
	return t.triangle.perimeter()
}
```
### Overload Methods
In Go you can not have two functions with the same name, but it is possible to have a method with the same name as long as it is specific to the receiver you want to use.
In other words you could write the wrapper method.
```Go
func (t coloredTriangle) perimeter() int {
	return t.size * 3 * 2
}
```
If you want to call the `perimeter()` method from the `triangle` struct, you can access it explicitly:
```Go
func main() {
	t := coloredTriangle{triangle{3}, "blue"}
	fmt.Println("Size:", t.size)
	fmt.Println("Perimaeter (colored)", t.perimeter())
	fmt.Println("Perimeter (normal)", t.triangle.perimeter())
}
```
### Encapsulation in Methods
Encapsulation means that a method is inaccessible to the caller (client) of an object, this is done by uncapitalizing the identifier or capitalizing the identifier to make a method public.
Encapsulation in Go takes effect only between packages, so you can only hode implementation details from other packages, no the package itself.

We create a new package `geometry` and move the triangle struct there:
```Go
package geometry
type Triangle struct {
	size int
}

func (t *Triangle) doubleSize() {
	t.size *= 2
}

func (t *Triangle) SetSize(size int) {
	t.size = size
}

func (t *Triangle) Perimeter() int {
	t.doubleSize()
	return t.size * 3
}
```
You could use the package like this:
```Go
func main() {
	t := geometry.Triangle{}
	t.SetSize(3)
	fmt.Println("Perimeter", t.Perimeter())
}
```
If you try to call the `doubleSize()` method from the `main()` function the program will panic
```Go
func main() {
	t :=geometry.Triangle{}
	t.SetSize(3)
	fmt.Println("Size", t.size)
	fmt.Println("Perimeter", t.Perimeter())
}
```
## Interfaces
In Go interfaces are an abstract type of data that is used to represent the behavior of other types, they are like a blueprint or contract, which includes the methods, that an object should possess or implement.
Unlike in other programming languages, interfaces are satisfied in Go implicitly, as it does not offer keywords to implement an interface.
>[!important]
>An interface type is a set of method signatures


### Declare an Interface
```Go
type Shape interface {
	Perimeter() float64
	Area() float64
}
```
Every type that is a `Shape` needs to have both the `Perimeter()` and `Area()` methods. 
### Implement an Interface
We create a `Square` struct that has the methods form the `Shape` interface
```Go
type Square struct {
	size float64
}

func (s Square) Area() float64 {
	return s.size * s.size
}

func (s Square) Perimeter() float64 {
	return s.size * 4
}
```
The `Square` struct matches the `Shape` interface method signature.

>[!important] 
>Go knows what interface a type is implementing at runtime when you are using it.

```Go
func main() {
	var s Shape = Square{3}
	fmt.Printf("%T\n", s)
	fmt.Println("Area: ", s.Area())
	fmt.Println("Perimeter:", s.Perimeter())
}
```
Let us create another type, such as `Circle` to further see why interfaces are useful.
```Go
type Circle struct {
	radius float64
}

func (c Circle) Area () float64 {
	return math.Pi * c.radius * c.raadius
}

func (c Circle) Perimeter() float64 {
	return 2 * math.Pi * c.radius
}
```
Now refactor the `main()` function and create a function that can print the type of the object it receives, as well as its area and perimeter
```Go
func print Information(s Shape) {
	fmt.Printf("%T\n", s)
	fmt.Println("Area: ", s.Area)
	fmt.Println("Perimeter:", s.Perimeter())
	fmt.Println()
}
```
With `Shape` as a parameter, you could send a `Square` or `Circle` object and it will work, although the output will be different.

The `main()` function should now look like this:
```Go
func main() {
	var s Shape = Square{3}
	printInformation(s)

	c := Circle{6}
	printInformation(c)
}
```
>[!note]
>We do not specify that it is a `Shape` object. However, the `printInformation` function expects an object that implements the methods that are defined in the `Shape` interface.
>
### Implement a Stringer interface
An example of extending functionality is to use a `Stringer`, an `Interface` that has a `String()` method
```Go
type Stringer interface {
	String() string
}
```
The `fmt.Printf` function uses this interface to print values, which means you can write custom `String()`
```Go 
package main

import "fmt"

type Person struct {
	Name, Country string
}

func (p Person) String() string {
	return fmt.Sprint("%v is from %v", p.Name, p.Country)
}

func main() {
	rs := Person{"John Doe", "USA"}
	ab := Person{"Mark Collins", "United Kingdom"}
	fmt.Printf("%s\n%s\n", rs, ab)
}
```
### Write a custom Server API
>[!note]
>More information on writing [Web Applications](https://golang.org/doc/articles/wiki)

Typically you would write a web server by using the `http.Handler` interface from the `net/http` package
```Go
package http

type Handler interface {
	ServeHTTP(w ResponseWriter, r *Request)
}

func ListenAndServe(address string, h Handler) error
```
Notice how the `ListenAndServe` function is expecting a server address, such as `http://localhost:8000` and an instance of the `Handler` that dispatches the response from the call to the server address.

Let us create and explore a program
```Go
package main

import (
	"fmt"
	"log"
	"net/http"
)

type dollars float32

func (d dollars) String() string {
	return fmt.Sprintf("$%.2f", d)
}

type database map[string]dollars

func (db database) ServerHTTP(w http.ResponseWriter, req *http.Request) {
	for item, price := range db {
		fmt.Fprintf(w, "%s: %s\n", item, price)
	}
}

func main() {
	db := database{"Go T-Shirt": 25, "Go Jacket": 55}
	log.Fatal(http.ListenAndServe("localhost:8000", db))
}
```
>[!note]
>If you run this program now you should not get any output

After you run the program open `http://localhost:8000` in your browser or run this command in a terminal
```Bash
curl http://localhost:8000
```
You should now get this output
```
Go T-Shirt: $25.00
Go Jacket: $55.00
```
#### Program Explanation
1. We first create a custom `float32` type and a custom `String()` method
```Go
type dollars float32

func (d dollars) String() string {
	return fmt.Sprintf("$.2f", d)
}
```
2. We write an implementation of the `ServeHTTP` method that the `http.Handler` can use. 
   We then create a custom map and write the `ServeHTTP` method by using the `database` type as the receiver.
```Go
type database map[string]dollars

func (db database) ServeHTTP(w http.ResponseWriter, req *http.Request) {
	for item, price := range db {
		fmt.Fprintf(w, "%s: %s\n", item, price)
	}
}
```
3. In the `main()` function, instantiate and initialize a `database` type with some values. We also start the HTTP server by using the `http.ListenAndServe` function, including the port to use and the `db` object that implements a custom version of the `ServeHTTP` method. When you run the program, Go uses your implementation of that method.
```Go
func main() {
	db := database{"Go T-Shirt": 25, "Go Jacket": 55}
	log.Fatal(http.ListenAndServe("localhost:8000", db))
}
```
### Exercise: Create a Package to manage an Online Store

The challenge includes the following elements:
1. Create a custom type called `Account` that includes the first and last name of the account owner, as well as the functionality to `ChangeName`. 


Here is the code for the `store` package
```Go
package store

import (
	"errors"
	"fmt"
)

type Account struct {
	FirstName string
	LastName string
}

type Employee struct {
	Account
	Credits float64
}

func (a *Account) ChangeName(newname string) {
	a.FirstName = newname
}

func CreateEmployee(firstName, lastName string, credits float64)(*Employee, error) {
	return &Employee{Account{firstName, lastName}, credits}, nil
}

func (e *Employee) AddCredits(amount float64) (float64, error) {
	if amount > 0.0 {
		if amount <= e.Credits {
			e.Credits += amount
			return e.Credits, nil
		}
		return 0.0, errors.New("You can not remove more credits)
	}
	return 0.0, errors.New("You can not remove negative numbers")
}

func (e *Employee) CheckCredits() float64 {
	return e.Credits
}	   
```

Here is the code for the main program
```Go
package main

import(
	"fmt"
	"store"
)

func main() {
	bruce, _ := store.CreateEmployee("Bruce", "Lee", 500)
	fmt.Println(bruce.CheckCredits())
	credits, err := bruce.AddCredits(250)
	if err != nil {
		fmt.Println("Error:", err)
	}
	else {
		fmt.Println("New Credits Balance = ", credits)
	}

	_, err = bruce.RemoveCredits(2500)
	if err != nil {
		fmt.Println("Can not withdraw or oberdraw!", err)
	}

	bruce.ChangeName("Mark")

	fmt.Println(bruce)
}
```

# Concurrency
## Goroutines and channels
*Goroutines* are a feature that can be run concurrently to the main program or other goroutines. They are managed by the Go runtime, where they are mapped and moved to the appropriate operating system thread and garbage-collected when no longer needed.

When multiple processor cores are available, goroutines can be run in parallel because various threads are running on different processing cores.
In a single thread, Go handles the context switching and concurrent orchestration for you.
![[Goroutine.png]]

*Channels* enable two goroutines to communicate with each other or another process. 
![[Channel.png]]

Using channels and goroutines together provides functionality similar to lightweight threads or internal services that communicate over a type-defined API.

>[!tip]
>Think about channels as an external listener that can tun concurrently to the rest of your code.
>You can than start to mentally map how to keep concurrent and/or asynchronous processes running alongside your linear code.

# Building CLI applications 
