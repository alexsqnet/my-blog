---
title: Facade - Design Pattern
date: 2022-12-20 23:13:12
tags:
    - design pattern
category:
    - design pattern

thumbnail: uploads/facade-design-pattern/cover.png
banner: uploads/facade-design-pattern/cover.png
---
![Alt Text](uploads/facade-design-pattern/cover.png)
# What is the Facade Design Pattern?
**The Facade design pattern** is a structural pattern that provides a simplified interface to a complex system of classes, libraries, or frameworks. It hides the complexities of the system and makes it easier to use by providing a unified, high-level interface.

<!--more-->

# Metaphor
![Alt Text](uploads/facade-design-pattern/metaphor-facade.png)
In this image, we see an interface through which we can easily drive the car. The driver, through this high-level interface, interacts with other complicated subsystems (engine, other components). The driver may not know these subsystems, but can use this high-level interface, which represents the Facade, to drive the car.

# UML Diagram
![Alt Text](uploads/facade-design-pattern/UML.png)

# Advantages of Using the Facade Pattern
1. **Simplified Interface**: The Facade pattern provides a single, easy-to-use interface for complex subsystems.
2. **Reduces Complexity**: By hiding the intricate details of the subsystems, it reduces the learning curve and eases the use of the system.
3. **Decoupling**: It decouples the client code from the subsystem, making the code more modular and easier to maintain.
4. **Improved Code Readability**: Simplified interfaces improve the readability and manageability of the code.

# Implementing the Facade Pattern in C#
Consider a scenario where we have a complex system for managing a home theater. It consists of several subsystems: Amplifier, DVDPlayer, Projector, and Screen. Let's implement a Facade for this system.

![Alt Text](uploads/facade-design-pattern/home-theater.jpg)

## Subsystem classes
``` csharp
public class Amplifier
{
    public void On() => Console.WriteLine("Amplifier is on");
    public void Off() => Console.WriteLine("Amplifier is off");
    public void SetVolume(int level) => Console.WriteLine($"Amplifier volume set to {level}");
}

public class DVDPlayer
{
    public void On() => Console.WriteLine("DVD Player is on");
    public void Off() => Console.WriteLine("DVD Player is off");
    public void Play(string movie) => Console.WriteLine($"Playing movie: {movie}");
}

public class Projector
{
    public void On() => Console.WriteLine("Projector is on");
    public void Off() => Console.WriteLine("Projector is off");
    public void WideScreenMode() => Console.WriteLine("Projector in widescreen mode");
}

public class Screen
{
    public void Down() => Console.WriteLine("Screen is down");
    public void Up() => Console.WriteLine("Screen is up");
}

```

## Facade class
```csharp
public class HomeTheaterFacade
{
    private readonly Amplifier _amplifier;
    private readonly DVDPlayer _dvdPlayer;
    private readonly Projector _projector;
    private readonly Screen _screen;

    public HomeTheaterFacade(Amplifier amplifier, DVDPlayer dvdPlayer, Projector projector, Screen screen)
    {
        _amplifier = amplifier;
        _dvdPlayer = dvdPlayer;
        _projector = projector;
        _screen = screen;
    }

    public void WatchMovie(string movie)
    {
        Console.WriteLine("Get ready to watch a movie...");
        _screen.Down();
        _projector.On();
        _projector.WideScreenMode();
        _amplifier.On();
        _amplifier.SetVolume(5);
        _dvdPlayer.On();
        _dvdPlayer.Play(movie);
    }

    public void EndMovie()
    {
        Console.WriteLine("Shutting movie theater down...");
        _dvdPlayer.Off();
        _amplifier.Off();
        _projector.Off();
        _screen.Up();
    }
}
```

## Using the Facade
```csharp
class Program
{
    static void Main(string[] args)
    {
        Amplifier amplifier = new Amplifier();
        DVDPlayer dvdPlayer = new DVDPlayer();
        Projector projector = new Projector();
        Screen screen = new Screen();

        HomeTheaterFacade homeTheater = new HomeTheaterFacade(amplifier, dvdPlayer, projector, screen);

        homeTheater.WatchMovie("Predator");
        // Output:
        // Get ready to watch a movie...
        // Screen is down
        // Projector is on
        // Projector in widescreen mode
        // Amplifier is on
        // Amplifier volume set to 5
        // DVD Player is on
        // Playing movie: Predator

        homeTheater.EndMovie();
        // Output:
        // Shutting movie theater down...
        // DVD Player is off
        // Amplifier is off
        // Projector is off
        // Screen is up
    }
}
```
## Conclusion
The Facade design pattern is an excellent way to manage complexity in large systems by providing a simplified interface. In C#, the Facade pattern can significantly enhance code readability and maintainability, making it easier to interact with complex subsystems. By using the Facade pattern, you can create more modular, decoupled, and easy-to-use systems.