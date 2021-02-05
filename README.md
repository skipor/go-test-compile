# go-test-compile

`go-test-compile` compiling all golang tests without running. 
It can be used like `go build` that also build tests or like `go test -c` that works for multiple packages.

### Why it is better than `go test --count=0` or `--run='^$'`

* It does not run any TestMain.
* It can save resulting binaries.

## How it is implemented

Idea is to use `go test --exec` flag. Here it's doc from `go help test`:

```
	-exec xprog
	    Run the test binary using xprog. The behavior is the same as
	    in 'go run'. See 'go help run' for details.
```

* If pass `--exec` that does `exit 0`, than it works like  _build all binaries and don't save them_.
* If pass `--exec` that moves binary somewhere, than works like _Build all binaries and do save them (maybe in a directory tree)_.

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
