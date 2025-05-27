# Events and delegates

### Introduction

The `event` keyword allows objects to communicate effectively by providing a built-in mechanism on top of delegates. Delegates define the method signature that subscribers must implement, while events enable publishers to notify these subscribers when something significant occurs. Although initially challenging for many developers, mastering these concepts greatly enhances your application's flexibility, maintainability, and overall design quality.

### Why do we need events?

Events help us build loosely coupled applications. In loosely coupled applications, components or classes aren't tightly bound to each other. This allows you to extend or modify your application easily without breaking existing functionalities.

### Problem

Consider a scenario where you have a class that encodes videos and, upon completion, needs to notify users via email. Initially, you might write a direct call to an email service within your video encoder method. However, if later you need to add more notification methods, like text messaging, you must modify the encoder class each time. This tight coupling creates a dependency where changes to notification logic require recompiling and redeploying multiple classes, increasing complexity and the risk of errors.

```csharp
public class Video
{
    public string Title { get; set; }
}

public class VideoEncoder
{
    public void Encode(Video video)
    {
        Console.WriteLine("Encoding Video...");
        Thread.Sleep(3000);

        var mailService = new MailService();
        mailService.Send(new Mail());

        // If later we want to add text messaging:
        // var messageService = new MessageService();
        // messageService.Send(new TextMessage());
    }
}
```

This design is rigid and inflexible, requiring repeated modifications.

### Solution

Events solve this problem by clearly separating the "publisher" (the class that triggers an event) from the "subscribers" (classes that respond to the event). With events, the publisher, such as a `VideoEncoder`, doesn't directly depend on the specific notification methods like email or text messages. Instead, it simply announces that an event occurred (e.g., a video has been encoded), and interested subscribers handle the notification independently. This design allows you to add or remove subscribers without modifying the publisher.

### Steps to implement events

1. **Define a delegate**: A delegate specifies the method signature that subscribers must implement.
2. **Define an event**: Create an event based on the delegate.
3. **Raise the event**: Notify subscribers when something happens.

#### Option 1: Custom Delegate

Define a custom delegate explicitly:

```csharp
public delegate void VideoEncodedEventHandler(object source, EventArgs args);

public class VideoEncoder
{
    public event VideoEncodedEventHandler VideoEncoded;

    protected virtual void OnVideoEncoded()
    {
        VideoEncoded?.Invoke(this, EventArgs.Empty);
    }
}
```

#### Option 2: Using Built-in Delegates (`EventHandler<T>`)

Modern C# provides built-in delegates to simplify event creation:

```csharp
public class VideoEventArgs : EventArgs
{
    public Video Video { get; set; }
}

public class VideoEncoder
{
    public event EventHandler<VideoEventArgs> VideoEncoded;

    protected virtual void OnVideoEncoded(Video video)
    {
        VideoEncoded?.Invoke(this, new VideoEventArgs { Video = video });
    }
}
```

{% hint style="success" %}
Using built-in delegates like `EventHandler<T>` reduces boilerplate code and is recommended for most scenarios.
{% endhint %}

### Subscribers Example

```csharp
public class MailService
{
    public void OnVideoEncoded(object source, VideoEventArgs e)
    {
        Console.WriteLine($"MailService: Sending an email for video '{e.Video.Title}'.");
    }
}

public class MessageService
{
    public void OnVideoEncoded(object source, VideoEventArgs e)
    {
        Console.WriteLine($"MessageService: Sending a text message for video '{e.Video.Title}'.");
    }
}
```

### Wiring Subscribers to Publisher

```csharp
class Program
{
    static void Main()
    {
        var video = new Video { Title = "C# Tutorial" };
        var encoder = new VideoEncoder();

        var mailService = new MailService();
        var messageService = new MessageService();

        encoder.VideoEncoded += mailService.OnVideoEncoded;
        encoder.VideoEncoded += messageService.OnVideoEncoded;

        encoder.Encode(video);
    }
}
```

**Output:**

```bash
Encoding video 'C# Tutorial'...
MailService: Sending an email for video 'C# Tutorial'.
MessageService: Sending a text message for video 'C# Tutorial'.
```

This demonstrates adding new subscribers easily without changing the publisher.

### Partial Events (C# 14)

C# 14 introduces partial events, allowing you to separate event declaration from its implementation, enhancing readability and maintainability.

#### Partial Event Declaration:

```csharp
public partial class VideoEncoder
{
    public partial event EventHandler<VideoEventArgs> VideoEncoded;
}
```

#### Partial Event Implementation:

```csharp
public partial class VideoEncoder
{
    private EventHandler<VideoEventArgs> _videoEncoded;

    public partial event EventHandler<VideoEventArgs> VideoEncoded
    {
        add => _videoEncoded += value;
        remove => _videoEncoded -= value;
    }

    protected virtual void OnVideoEncoded(Video video)
    {
        _videoEncoded?.Invoke(this, new VideoEventArgs { Video = video });
    }
}
```

Partial events provide clear separation between event declarations and their implementations.

### Key Points

* Events promote loose coupling, allowing subscribers to handle notifications independently of publishers.
* Custom delegates explicitly define method signatures.
* Built-in delegates (`EventHandler<T>`) simplify event creation.
* Partial events (C# 14) clearly separate event declarations from their implementations.

### Conclusion

Using events, you can easily add subscribers without modifying publisher classes, greatly improving code quality and maintainability.

### FAQs

**Why not directly call subscriber methods instead of events?**\
Direct calls tightly couple classes, making future changes harder. Events decouple publishers and subscribers.

**When should I use custom delegates versus `EventHandler<T>`?**\
Prefer `EventHandler<T>` for simplicity. Use custom delegates only when built-in delegates don't meet specific needs.

**Can an event have multiple subscribers?**\
Yes, events are multicast and support multiple subscribers.

**What is the advantage of partial events?**\
They separate event declaration from accessor implementation, improving readability and maintainability, especially in large or generated codebases.
