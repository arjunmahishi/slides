---
title: Using the Stringer tool in Golang
author: "@arjunmahishi"
extensions: []
styles:
  margin:
    top: 2
---

# Using the striger tool in Golang

Name: Go meetup
Venue: Deepsource office
Date: 14th Oct 2022

---

# The Stringer interface

- It's an interface defined by the `fmt` package

- `fmt` and a lot of other packages look for implementations of this to print values

```go
type Stringer interface {
  String() string
}
```

---

# Example

```go
type StatusCode int

const (
  statusOK                StatusCode = 200
  statusInternalServerErr StatusCode = 500
)

func main() {
  fmt.Println(statusOK)
  fmt.Println(statusInternalServerErr)

  // Output:
  // 200
  // 500
}
```

---

# Example

```go
type StatusCode int

func (c StatusCode) String() string {
  switch c {
  case 200:
    return "Status OK"
  case 500:
    return "Internal server error"
  default:
    return "Unknown status"
  }
}

const (
  statusOK                StatusCode = 200
  statusInternalServerErr StatusCode = 500
)

func main() {
  fmt.Println(statusOK)
  fmt.Println(statusInternalServerErr)

  // Output:
  // Status OK
  // Internal server error
}
```
---

# Where is this useful ?

- Logging/Telemetry -> Numbers like 1, 2, 3 are not very useful while debugging


- Comparison with user input
  ```go
  status := req.UserInput("http_status") // string value
  if status == statusOK.String() {
    // do stuff
  }
  ```

---

# What is Stringer (the CLI tool) ?

- A CLI tool that auto-generate the `String()` method for a given `int` type

  ```bash
  go install golang.org/x/tools/cmd/stringer@latest
  ```

- Part of the `gopls`, `goimports`, `gofmt` gang

---

# Demo

---

# Challenge

What if I have multiple types and multiple sub-packages ?

Do I need to go and run `stringer` in all the sub-directories and for all types separately ?

---

# Go generate

A Go command that helps you run tools that generate code

It looks for special comments like this:

```go
package main 

//go:generate <command>

func main() {

}
```

ref: https://go.dev/blog/generate

---

# Demo

---

# Things to remember

- To run `go generate` as a part of your build/test/... workflows
  Because `go build` might NOT fail

  ```yaml
  test:
    go generate ./... && go test -v ./...

  build:
    go generate ./... && go build
  ```

- `go generate` does NOT guaranty the order of execution

---

# Thank you
