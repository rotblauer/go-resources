# Resources

Unfancy resources embedding with Go.

- No blings.
- No runtime dependency.
- Embeddable builder.

### Dude, Why?

Yes, there is quite a lot of projects that handles resource embeding but they come with more bling than you will probably ever need and you often ended up with having dependenies for your end project, not this time.

### Installing

Just go get it!

```sh
$ go get github.com/omeid/go-resources/cmd/resources
```


### Usage
```sh
$ resources -help
Usage of resources:
  -declare=false: Whether to declare the "var" or not.
  -output="": The filename to write the output to.
  -package="main": The name of package to generate.
  -tag="": The tag to use for the generated package. Use empty for no tag.
  -trim="": Path prefix to remove from the resulting file path
  -var="FS": The name of variable to assign the virtual-filesystem to.
```

#### Techniques for "live" resources for development

```go
package main

import "net/http"
var Assets http.FileSystem 


func main() {
//Use Assets here
}
```

```go
// +build !embed

package main

import "net/http"

var Assets = http.Dir("./public")
```
Now when you build or run your project, you will have files directly served from `./public` directory. This is pretty helpful for development.

To embed your resources, do

```sh
$ resources -output="public_resources.go" -var="Assets" -tag="embed" public/*
$ go build -tags=embed
```

Now your resources should be embeded with your program!



### Optimization
Generating resources result in a very high number of lines of code, 1mb of resources result about 5mb of code at over 87 thousand lines of code, _don't worry, the size of data stored in your binary is exactly same as the resources (eg. 1mb of resources only increases your binary size by 1mb_, compiling this many lines of code takes time and slows down the compiler.
To avoid recompiling the resources everyi time and leverage the `go build` cache, generate your resources into a standalone package and then import it, this will allow for faster iteration as you don't have to wait for the resources to be compiled with every change.

### Go Generate
There is a few reasons to avoid resource embeding in Go generate,
first Go Generate is for generating Go source code from your code, generally the resources you want to embed aren't effected by the Go source directly and as such generating resources are out of the scope of Go Generate.
Second, You're unnecessarily slowing down code iterations by blocking `go generate` for resource generation.

But if you must use, put the `//go:generate resources` followed by the usual flags on the command-line somewhere in your Go files.

### Contributing
Please consider opening an issue first, or just send a pull request. :)

### Credits
See [Contributors](https://github.com/omeid/go-resources/graphs/contributors).

### LICENSE
  [MIT](LICENSE).


### TODO

 - Add tests. 
 - Remove "net/http" dependency.
