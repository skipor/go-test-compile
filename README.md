# go-test-compile

Compiling all golang tests without running. It can be used like `go build` that also build tests 
or like `go test -c` that works for multiple directories.

## Install

```shell
# Download somewhere in PATH. 
GO_TEST_COMPILE="$(go env GOPATH)/bin/go-test-compile"
curl -s https://raw.githubusercontent.com/skipor/go-test-compile/main/go-test-compile > "${GO_TEST_COMPILE:?}"
chmod +x "${GO_TEST_COMPILE:?}"
# Check that executable in PATH.
go-test-compile --help
#> go-test-compile wraps 'go test' allowing not run test executables at all (even TestMain)
#> and save compiled test executables for future run.
#> 
#> Usage:
#>   go-test-compile [-o|--output <output directory>] [go test flags and arguments]
```

## Usage

```shell
# Download some packages to test.
go get -d -t golang.org/x/tools/...
# Just check that all code building. Like `go build ./...` but also build test packages.
go-test-compile golang.org/x/tools/godoc/...  

# Compile all test binaries. # Like `go test -c ./...` it it were implemented.
go-test-compile -o /tmp/test-executables golang.org/x/tools/cmd/godoc golang.org/x/tools/godoc/...
tree /tmp/test-executables
#> /tmp/test-executables
#> └── golang.org
#>     └── x
#>         └── tools
#>             ├── cmd
#>             │   └── godoc.test
#>             ├── godoc
#>             │   ├── redirect.test
#>             │   ├── static.test
#>             │   ├── vfs
#>             │   │   ├── gatefs.test
#>             │   │   ├── mapfs.test
#>             │   │   └── zipfs.test
#>             │   └── vfs.test
#>             └── godoc.test
```
