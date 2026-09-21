# Building, Installing, and Testing Go Programs

Once a module and its packages exist (see
[Creating packages and modules](creating_and_accessing_pkgs.md)), the `go`
tool covers the rest of the day-to-day workflow: compiling, installing,
fetching dependencies, and running tests.

## `go build` vs `go install`

```bash
$ cd $HOME/hello/morestrings
$ go build
```

`go build` compiles a package and caches the result locally — for a
non-`main` package this produces no output file, it just proves the code
compiles.

```bash
$ go install example/user/hello
```

`go install` builds the `main` package into an executable binary and
installs it. The install location is controlled by `GOBIN` if set,
otherwise the `bin` subdirectory of `GOPATH` (default `$HOME/go/bin`):

```bash
$ go env -w GOBIN=/somewhere/else/bin   # set a default
$ go env -u GOBIN                       # unset it again
```

Within a module's working directory, these are equivalent:

```bash
$ go install example/user/hello
$ go install .
$ go install
```

## Fetching remote dependencies

An import path can point at a real repository. Importing it and running
`go mod tidy` downloads the module and records the version in `go.mod`:

```go
import "github.com/google/go-cmp/cmp"
```

```bash
$ go mod tidy
go: finding module for package github.com/google/go-cmp/cmp
go: found github.com/google/go-cmp/cmp in github.com/google/go-cmp v0.5.4
```

`go mod tidy` also removes requirements for modules that are no longer
used. Downloaded modules are cached under `pkg/mod` in `GOPATH` and marked
read-only since they're shared across every module that requires that
version. `go clean -modcache` clears the cache entirely.

## Testing with `go test`

A test file ends in `_test.go` and contains `TestXXX(t *testing.T)`
functions. The test framework calls each one; a call to `t.Error` or
`t.Fail` marks that test as failed.

```go
package morestrings

import "testing"

func TestReverseRunes(t *testing.T) {
    cases := []struct {
        in, want string
    }{
        {"Hello, world", "dlrow ,olleH"},
        {"Hello, 世界", "界世 ,olleH"},
        {"", ""},
    }
    for _, c := range cases {
        got := ReverseRunes(c.in)
        if got != c.want {
            t.Errorf("ReverseRunes(%q) == %q, want %q", c.in, got, c.want)
        }
    }
}
```

```bash
$ go test
PASS
ok      example/user/hello/morestrings 0.165s
```

No test runner setup or registration is needed — `go test` discovers
`_test.go` files in the current package automatically.

Source: [How to Write Go Code — go.dev](https://go.dev/doc/code#next)
