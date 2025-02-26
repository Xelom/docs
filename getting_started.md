# Getting started

[![](https://img.shields.io/github/release/gofiber/fiber)](https://github.com/gofiber/fiber/releases) ![](https://img.shields.io/github/languages/top/gofiber/fiber) [![](https://godoc.org/github.com/gofiber/fiber?status.svg)](https://godoc.org/github.com/gofiber/fiber) ![](https://goreportcard.com/badge/github.com/gofiber/fiber) [![GitHub license](https://img.shields.io/github/license/gofiber/fiber.svg)](https://github.com/gofiber/fiber/blob/master/LICENSE) [![Join the chat at https://gitter.im/gofiber/community](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/gofiber/community)

**[Fiber](https://github.com/gofiber/fiber)** is an [Express.js](https://expressjs.com/en/4x/api.html) styled HTTP web framework implementation running on [Fasthttp](https://github.com/valyala/fasthttp), the **fastest** HTTP engine for Go (Golang). The package make use of **similar framework convention** as they are in Express.

People switching from [Node.js](https://nodejs.org/en/about/) to [Go](https://golang.org/doc/) often end up in a bad learning curve to start building their webapps, this project is meant to **ease** things up for **fast** development, but with **zero memory allocation** and **performance** in mind.

#### Installing

Assuming you’ve already installed [Go](https://golang.org/doc/), install the [Fiber](https://github.com/gofiber/fiber) package by calling the following command:

```shell
go get -u github.com/gofiber/fiber
```

#### Hello world

Embedded below is essentially the simplest Fiber app you can create.

```shell
create server.go
```

```go
package main

import "github.com/gofiber/fiber"

func main() {
  app := fiber.New()

  app.Get("/", func(c *fiber.Ctx) {
    c.Send("Hello, World!")
  })

  app.Listen(8080)
}
```

```shell
go run server.go
```

Browse to http://localhost:8080 and you should see Hello, World! on the page.

#### Basic routing

Routing refers to determining how an application responds to a client request to a particular endpoint, which is a URI (or path) and a specific HTTP request method (GET, POST, and so on).

Each route can have one handler function, that is executed when the route is matched.

Route definition takes the following structures:

```go
// Function signature
app.Method(func(*fiber.Ctx))
app.Method(path string, func(*fiber.Ctx))
```

- **app** is an instance of **[Fiber](#hello-world)**.
- **Method** is an [HTTP request method](application?id=methods), in capitalization: Get, Put, Post etc
- **path string** is a path on the server.
- **func(\*fiber.Ctx)** is a function containing the [Context](/context) executed when the route is matched.

This tutorial assumes that an instance of fiber named app is created and the server is running. If you are not familiar with creating an app and starting it, see the [Hello world](#hello-world) example.

The following examples illustrate defining simple routes.

```go
// Respond with Hello, World! on the homepage:
app.Get("/", func(c *fiber.Ctx) {
  c.Send("Hello, World!")
})

// Parameter
// http://localhost:8080/hello%20world
app.Post("/:value", func(c *fiber.Ctx) {
  c.Send("Post request with value: " + c.Params("value"))
  // => Post request with value: hello world
})

// Optional parameter
// http://localhost:8080/hello%20world
app.Get("/:value?", func(c *fiber.Ctx) {
  if c.Params("value") != "" {
    c.Send("Get request with value: " + c.Params("Value"))
    return // => Get request with value: hello world
  }
  c.Send("Get request without value")
})

// Wildcard
// http://localhost:8080/api/user/john
app.Get("/api/*", func(c *fiber.Ctx) {
  c.Send("API path with wildcard: " + c.Params("*"))
  // => API path with wildcard: user/john
})
```

#### Static files

To serve static files such as images, CSS files, and JavaScript files, replace your function handler with a file or directory string.

```go
// Function signature
app.Static(root string)
app.Static(prefix, root string)
```

For example, use the following code to serve images, CSS files, and JavaScript files in a directory named public:

```go
app.Static("./public")
```

Now, you can load the files that are in the public directory:

```shell
http://localhost:8080/hello.html
http://localhost:8080/js/jquery.js
http://localhost:8080/css/style.css
```

_Caught a mistake? [Edit this page on GitHub!](https://github.com/gofiber/docs/blob/master/getting_started.md)_
