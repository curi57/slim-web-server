# Slim Web Server

Slim is a minimal HTTP server built on top of `HttpListener`. The goal of this project is to expose a very small surface for creating routes and handling requests without the overhead of a full framework.

## Starting the server

Create an instance of `Slim` and call `Start` to begin listening for incoming connections. By default the server listens on port `8080`.

```csharp
var server = new Slim();
await server.Start();
```

You can also pass a different port when constructing `Slim`:

```csharp
var server = new Slim(port: 5000);
await server.Start();
```

## Registering routes

Use `AddRoute` to map a URL path to an implementation of `ISlimHandler`:

```csharp
server.AddRoute("/hello", new HelloHandler());
```

When a request matches the given path, the provided handler is executed.

## Implementing `ISlimHandler`

`ISlimHandler` only defines a single asynchronous `Handle` method that receives the incoming `HttpListenerRequest`.

```csharp
using System.Net;

class HelloHandler : ISlimHandler
{
    public async Task Handle(HttpListenerRequest request)
    {
        Console.WriteLine("Hello world!");
        await Task.CompletedTask;
    }
}
```

After registering an instance of this handler with `AddRoute`, requests to `/hello` will trigger the code above.

