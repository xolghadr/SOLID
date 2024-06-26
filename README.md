# SOLID Principles

SOLID is an acronym for five design principles in object-oriented programming and design. These principles help to create software that is easy to maintain, understand, and extend.

### Single Responsibility Principle (SRP)
A class should have only one reason to change. This means that a class should only have one responsibility or job.

Example:

```mermaid
classDiagram
    class EmployeeSalaryCalculator {
        - CalculateSalary(Employee employee)
    }
    class EmployeeRepository {
        - SaveEmployee(Employee employee)
    }
    class Employee {
        - salary
        - name
    }
    EmployeeSalaryCalculator ..> Employee : uses
    EmployeeRepository ..> Employee : uses
```

```csharp
// Bad
public class Employee
{
    public void CalculateSalary()
    {
        // ...
    }

    public void SaveEmployee(Employee employee)
    {
        // ...
    }
}

// Good
public class EmployeeSalaryCalculator
{
    public double CalculateSalary(Employee employee)
    {
        // ...
    }
}

public class EmployeeRepository
{
    public void SaveEmployee(Employee employee)
    {
        // ...
    }
}
```

### Open-Closed Principle (OCP)
Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means that you should be able to add new functionality without changing the existing code.

Example:

```mermaid
classDiagram
    class Shape {
        <<abstract>>
        - CalculateArea()
    }
    class Rectangle {
        - Width
        - Height
        - CalculateArea()
    }
    class Circle {
        - Radius
        - CalculateArea()
    }
    Shape <|-- Rectangle
    Shape <|-- Circle
```

```csharp
// Bad
public class Rectangle
{
    public double Width { get; set; }
    public double Height { get; set; }

    public double CalculateArea()
    {
        return Width * Height;
    }
}

public class Circle
{
    public double Radius { get; set; }

    public double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
}

// Good
public abstract class Shape
{
    public abstract double CalculateArea();
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public override double CalculateArea()
    {
        return Width * Height;
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }

    public override double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
}
```
### Liskov Substitution Principle (LSP)
Subtypes must be substitutable for their base types. This means that if a class is a subtype of another class, it should be able to be used in the same way as the base class without any issues.

Example:

```mermaid
classDiagram
    class Bird {
        <<abstract>>
        - Move()
    }
    class Penguin {
        - Move()
    }
    class Sparrow {
        - Move()
    }
    Bird <|-- Penguin
    Bird <|-- Sparrow
```

```csharp
// Bad
public class Bird
{
    public virtual void Fly()
    {
        // ...
    }
}

public class Penguin : Bird
{
    public override void Fly()
    {
        // Penguins can't fly
    }
}

// Good
public abstract class Bird
{
    public abstract void Move();
}

public class Penguin : Bird
{
    public override void Move()
    {
        // Penguins can't fly, but they can swim
    }
}

public class Sparrow : Bird
{
    public override void Move()
    {
        // Sparrows can fly
    }
}
```
### Interface Segregation Principle (ISP)
Clients should not be forced to depend on interfaces they do not use. This means that you should create specific interfaces for specific clients instead of creating one large interface that contains all the methods.

Example:

```mermaid
classDiagram
    class IWorker {
        <<interface>>
        - Work()
    }
    class IEatable {
        <<interface>>
        - Eat()
    }
    class ISleepable {
        <<interface>>
        - Sleep()
    }
    class Worker {
        - Work()
        - Eat()
    }
    class SuperWorker {
        - Work()
        - Eat()
        - Sleep()
    }
    Worker ..> IWorker
    Worker ..> IEatable
    SuperWorker ..> IWorker
    SuperWorker ..> IEatable
    SuperWorker ..> ISleepable
```

```csharp
// Bad
public interface IWorker
{
    void Work();
    void Eat();
}

public class Worker : IWorker
{
    public void Work()
    {
        // ...
    }

    public void Eat()
    {
        // ...
    }
}

public class SuperWorker : IWorker
{
    public void Work()
    {
        // ...
    }

    public void Eat()
    {
        // ...
    }

    public void Sleep()
    {
        // ...
    }
}

// Good
public interface IWorker
{
    void Work();
}

public interface IEatable
{
    void Eat();
}

public class Worker : IWorker, IEatable
{
    public void Work()
    {
        // ...
    }

    public void Eat()
    {
        // ...
    }
}

public class SuperWorker : IWorker, IEatable, ISleepable
{
    public void Work()
    {
        // ...
    }

    public void Eat()
    {
        // ...
    }

    public void Sleep()
    {
        // ...
    }
}
```
### Dependency Inversion Principle (DIP)
High-level modules should not depend on low-level modules. Both should depend on abstractions. This means that you should depend on abstractions instead of concrete classes.

Example:
```mermaid
classDiagram
    class IOrderRepository {
        <<interface>>
        - SaveOrder(Order order)
    }
    class OrderRepository {
        - SaveOrder(Order order)
    }
    class OrderProcessor {
        - ProcessOrder(Order order)
    }
    OrderProcessor ..> IOrderRepository
    OrderRepository ..> IOrderRepository
```

```csharp
// Bad
public class OrderProcessor
{
    private readonly OrderRepository _orderRepository;

    public OrderProcessor()
    {
        _orderRepository = new OrderRepository();
    }

    public void ProcessOrder(Order order)
    {
        // ...
    }
}

// Good
public interface IOrderRepository
{
    void SaveOrder(Order order);
}

public class OrderRepository : IOrderRepository
{
    public void SaveOrder(Order order)
    {
        // ...
    }
}

public class OrderProcessor
{
    private readonly IOrderRepository _orderRepository;

    public OrderProcessor(IOrderRepository orderRepository)
    {
        _orderRepository = orderRepository;
    }

    public void ProcessOrder(Order order)
    {
        // ...
        _orderRepository.SaveOrder(order);
    }
}
```
### Conclusion

SOLID principles are a set of guidelines for designing software that is easy to maintain, understand, and extend. By following these principles, you can create software that is more flexible, reusable, and testable.

In this notes, we have covered each of the five SOLID principles with examples in C#, also provided class-diagrams.

By applying these principles in your software design, you can create software that is more robust and less prone to errors. While it may take some time to learn and apply these principles, the benefits are well worth the effort.

**Remember, SOLID principles are not rules, but guidelines.** They are meant to help you make informed decisions about your software design. Use them wisely and you will create software that is easier to maintain, understand, and extend.
