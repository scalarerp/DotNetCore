# DotNetCore.RabbitMQ

## Connection

```cs
public sealed class Connection
{
    public Connection(string hostName, int port, string userName, string password) { }

    public string HostName { get; }

    public int Port { get; }

    public string UserName { get; }

    public string Password { get; }
}
```

## IQueue<T>

```cs
public interface IQueue<T>
{
    void Publish(T obj);

    void Subscribe(Action<T> action);
}
```

## Queue<T>

```cs
public abstract class Queue<T> : IQueue<T>
{
    protected Queue(Connection connection) { }

    public void Publish(T obj) { }

    public void Subscribe(Action<T> action) { }
}
```

## Example

### Message

```cs
public class Product
{
    public string Name { get; set; }
}
```

### Queue

```cs
public interface IProductQueue : IQueue<Product> { }
```

```cs
public class ProductQueue : Queue<Product>, IProductQueue
{
    public ProductQueue(Connection connection) : base(connection) { }
}
```

### Publisher

```cs
var product = new Product { Name = "Product" };

IProductQueue productQueue = new ProductQueue(new Connection("localhost", 5672, "admin", "P4ssW0rd!"));

productQueue.Publish(product);
```

### Subscriber

```cs
IProductQueue productQueue = new ProductQueue(new Connection("localhost", 5672, "admin", "P4ssW0rd!"));

productQueue.Subscribe(product => Handle(product.Name));
```
