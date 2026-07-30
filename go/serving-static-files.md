# Serving Static Files from a Subpath in Go

When using `http.FileServer` in Go's `net/http` package, the directory
passed to `http.Dir` is the filesystem root. The URL path it's mounted at
gets passed through — so mounting `http.FileServer(http.Dir("./static"))` at
`/static/` would look for files at `./static/static/install.sh`.

Use `http.StripPrefix` to remove the URL prefix before passing the request
to the file server:

```go
http.Handle("/static/", http.StripPrefix("/static/", http.FileServer(http.Dir("./static"))))
```

Now `http://localhost:8080/static/install.sh` serves `./static/install.sh`.
