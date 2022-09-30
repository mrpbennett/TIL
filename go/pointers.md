# Pointers basics

A point is a way to reference a variable via it's position in memory...an
example

```go
var name = "Paul"
fmt.Printf("memory address of name is: %v \n", &name)
fmt.Println(name)


// prints...

memory address of name is: 0x14000010270
Paul
```

With the `fmt.Printf` line we're referenceing `name` with a pointer which is `&`
this allows us to reference `name` and it's stored value via its memory
location. As you can see via the print out
