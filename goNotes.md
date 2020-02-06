## On the Go language

### Tour of Go
Check out the [Tour of Go](https://tour.golang.org/list) which provides a walkthrough of language constructs along with a sandbox.

### Best practices
Some [best practices](https://talks.golang.org/2013/bestpractices.slide#3) by a Google Gopher
1. Avoid nesting by handling errors first
2. Avoid repetition when possible
3. Important code goes first
4. Document your code
5. Shorter is better
6. Packages with multiple files
7. Make your packages "go get"-able
8. Ask for what you need
9. Keep independent packages independent
10. Avoid concurrency in your API
11. Use goroutines to manage state
12. Avoid goroutine leaks

### Installation
// install go
brew install go

// add gopath to .bashrc, where go looks for projects
export GOPATH="~/go"
// ensure downloads via 'go get' are automatically made available
export PATH="$PATH:$GOPATH/go/bin"

// installing extra versions (not tested)
// alternatively, see gvm (go version manager)
go get golang.org/dl/go1.10.7
go1.10.7 download
// use newly installed version
go1.10.7
go version go1.10.7 linux/amd64

### Some commands
// download dependencies
go mod download
// verify dependencies
go mod verify

// build and run project
go build
./your_program

### Golang things
> Should you capitalize the first letter of your struct?
```go
type User struct {
	Uid			int
	Username, Department	string
}
```
Capitalize if the identifier may be accessed by another package.
Identifiers are exported when
- first character is an upper case letter
- declared in the package block, or if it is a field or a method name

> How to pretty print a struct, with variable names?
import "fmt"
fmt.Printf("%+v\n", rollbackClient)
