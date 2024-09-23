# FastEndpoints in .NET 8.0

## Why Use FastEndpoints?
FastEndpoints offers several benefits that make it an attractive choice for building .NET Core Web APIs:

+ **Simplicity**: FastEndpoints promotes a clear and concise structure for your API logic, reducing the need for boilerplate code and unnecessary abstractions.
+ **Performance**: With a focus on minimizing overhead, FastEndpoints is designed to deliver high-performance APIs that can handle a large number of requests efficiently.
+ **Modularity**: FastEndpoints encourages modular design, making it easy to organize and maintain your codebase. Each endpoint is self-contained, leading to better separation of concerns.
+ **Built-in Features**: FastEndpoints comes with built-in support for common API requirements, such as request validation, authentication, and error handling, allowing you to focus on business logic.

### Setup
```
dotnet new webapi -n FastEndpointsDemo
dotnet add package FastEndpoints
```

### Define Your First Endpoint
```
using FastEndpoints;

public class GetProductsEndpoint : EndpointWithoutRequest<List<Product>>
{
    public override void Configure()
    {
        Verbs(Http.GET);
        Routes("/products");
        AllowAnonymous();
    }

    public override async Task HandleAsync(CancellationToken ct)
    {
        var products = new List<Product>
        {
            new Product { Id = 1, Name = "Laptop", Price = 1200.99M },
            new Product { Id = 2, Name = "Smartphone", Price = 699.99M }
        };

        await SendAsync(products);
    }
}

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

### Register FastEndpoints
```
var builder = WebApplication.CreateBuilder(args);

// Register FastEndpoints
builder.Services.AddFastEndpoints();

var app = builder.Build();

// Use FastEndpoints
app.UseFastEndpoints();

app.Run();
```
