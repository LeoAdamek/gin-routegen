Gin RouteGen
============

A program to generate routing for Gin applications using
a simple text-based routing file.

Installation
------------

Install with:

    go get -u github.com/leoadamek/gin-routegen
    
Usage:
------

See `gin-routegen -h`

You can integrate the generation into `go generate` by specifying the generator in
a source file.

~~~~go
    //go:generate gin-routegen -i routes.txt -o routes.go -p main -f createRoutes
~~~~

Routing File Syntax
-------------------

Route files have a simple syntax.
Here's an example program.

    GET /pages/:page_id     handlers.Pages.ShowPage
    GET /users/sign_in      handlers.Sessions.New
    POST /users/sign_in     handlers.Sessions.Create
    DELETE /users/sign_out  handlers.Sessions.Delete
    OPTIONS *               handlers.Options
    RESOURCE /posts         handlers.PostResource
    
Each line contains an HTTP method, a Gin routeing spec and a path to a handler func.
The special pseudo-method `RESOURCE` will automatically create the following routings:

    RESROUCE /foo handlers.FooResource
    
    
    GET    /foo            handlers.FooResource.Index
    GET    /foo/:id        handlers.FooResource.Show
    PUT    /foo            handlers.FooResource.Create
    POST   /foo            handlers.FooResource.Create
    GET    /foo/new        handlers.FooResource.New
    GET    /foo/:id/edit   handlers.FooResource.Edit
    POST   /foo/:id        handlers.FooResource.Update
    PATCH  /foo/:id        handlers.FooResource.Update
    DELETE /foo/:id        handlers.FooResource.Delete
    POST   /foo/:id/delete handlers.FooResource.Delete
    
While using `PUT`, `PATCH` and `DELETE` methods where possible is reccommended,
`POST` handlers are included for clients and applications which do not readily
support using these semantic methods.

A resource must implement the `Resource` interface:

````go
    type Resource interface {
        func Index(*gin.Context)
        func Show(*gin.Context)
        func New(*gin.Context)
        func Edit(*gin.Context)
        func Create(*gin.Context)
        func Update(*gin.Context)
        func Delete(*gin.Context)
    }
````

Coming Soon™
------------

* Support for specifying middleware in route definition files
* Handling of including required imports (for now just run goimports on the generated sources)
