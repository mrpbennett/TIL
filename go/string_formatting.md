# String formatting in Go

Coming from Python I have gotten very use to f strings. Here is how you can achieve similar in Go.

Using the `fmt` package with the method [`Sprintf`](https://pkg.go.dev/fmt#Sprintf)

Lets take a python example first:

```python
businessrule_id: int = 652897451
url = f"https://gateway.company.com/api/business-rule/{businessrule_id}"
```

The same can be achieved with Go using the Sprintf method.

```go
businessrule_id := 12345567
url := fmt.Sprintf("https://gateway.company.com/api/business-rule/%s", businessrule_id)
```
