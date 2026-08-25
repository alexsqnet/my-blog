---
title: Configuration in ASP.NET Core applications
date: 2024-12-21 21:49:35
tags: [.net, asp.net core, configurations]
category: [".net"]

---

Configuration is a core part of every ASP.NET Core application.  
Before comparing `IOptions`, `IOptionsSnapshot`, and `IOptionsMonitor`, let’s first understand **how configuration classes are created and registered**.

> `IOptions` vs `IOptionsSnapshot` vs `IOptionsMonitor` in ASP.NET Core

<!--more-->

<br>

## Creating a configuration (Options) class

In ASP.NET Core, it is a best practice to map configuration sections from
`appsettings.json` to **strongly typed classes**.

### Example: appsettings.json

```json
{
  "Jwt": {
    "Issuer": "MyApi",
    "Audience": "MyApiUsers",
    "Secret": "super-secret-key"
  }
}
```

### Matching options class

The class structure should **match the JSON structure exactly**.

```csharp
public class JwtOptions
{
    public string Issuer { get; init; }
    public string Audience { get; init; }
    public string Secret { get; init; }
}
```

✔ Property names match JSON keys  
✔ Nested objects become nested classes  
✔ No logic — only configuration data  

---

<br>

## Registering the options in Program.cs

You only need **one line** in `Program.cs`:

```csharp
builder.Services.Configure<JwtOptions>(
    builder.Configuration.GetSection("Jwt"));
```

### Important
- This single line registers **all three mechanisms**:
  - `IOptions<JwtOptions>`
  - `IOptionsSnapshot<JwtOptions>`
  - `IOptionsMonitor<JwtOptions>`
- You **do not** register them separately.

> Configuration is registered once —  
> the choice of options type is made at injection time.

---

<br>

## The Big Picture

> **All three options types read the same configuration**,  
> but they differ in **lifetime**, **when values update**, and **where they can be used**.

---

<br>

## IOptions<T> — Static configuration

### What it is
`IOptions<T>` reads configuration **once**, when the application starts.

```csharp
public class TokenService
{
    private readonly JwtOptions _options;

    public TokenService(IOptions<JwtOptions> options)
    {
        _options = options.Value;
    }
}
```

### Key characteristics
- **Singleton**
- Values do **not change** until application restart
- Very stable and predictable

### When to use it
- JWT settings  
- Connection strings  
- API keys  
- Infrastructure configuration  

---

<br>

## IOptionsSnapshot<T> — Per request configuration

### What it is
`IOptionsSnapshot<T>` re-reads configuration **once per HTTP request**.

```csharp
public class AuthController : ControllerBase
{
    private readonly JwtOptions _options;

    public AuthController(IOptionsSnapshot<JwtOptions> options)
    {
        _options = options.Value;
    }
}
```

### Key characteristics
- **Scoped (per request)**
- Values update **on the next request**
- Configuration remains stable during one request
- Cannot be injected into Singleton services

### Important detail
If configuration changes **during a request**,  
that request will **not** see the new values.

The next request will.

### When to use it
- Web APIs  
- Feature flags  
- Tunable settings  
- Runtime configuration testing  

---

<br>

## IOptionsMonitor<T> — Live configuration

### What it is
`IOptionsMonitor<T>` listens for configuration changes **in real time**.

```csharp
public class BackgroundWorker
{
    public BackgroundWorker(IOptionsMonitor<JwtOptions> monitor)
    {
        monitor.OnChange(newOptions =>
        {
            // React immediately to configuration changes
        });
    }
}
```

### Key characteristics
- **Singleton**
- Values update immediately
- Supports `OnChange` event
- Can be injected into Singleton services

### When to use it
- Background services  
- Long-running workers  
- Cache refresh logic  
- Polling / watcher services  

---

<br>

## Key Difference (This Matters)

### IOptionsSnapshot
```
Request A  -> old values
Config changes
Request B  -> new values
```

### IOptionsMonitor
```
Config changes
OnChange fires immediately
All consumers see new values
```

---

<br>

## Comparison Table

| Feature | IOptions | IOptionsSnapshot | IOptionsMonitor |
|------|---------|-----------------|----------------|
| Lifetime | Singleton | Scoped | Singleton |
| Updates without restart | ❌ | ✅ (next request) | ✅ (live) |
| Has OnChange | ❌ | ❌ | ✅ |
| Works in Singleton | ✅ | ❌ | ✅ |
| Best for Web API | ⚠️ | ⭐⭐⭐ | ⚠️ |
| Best for Background Services | ❌ | ❌ | ⭐⭐⭐ |

---

<br>

## Why are there three options?

ASP.NET Core supports both:
- request-based applications (Web APIs)
- long-running services (workers)

Different lifetimes require different configuration strategies.

---

<br>

## The Golden Rule

> 🔒 **Static configuration → IOptions**  
> 🔁 **Request-based configuration → IOptionsSnapshot**  
> 🔄 **Live / always-on services → IOptionsMonitor**

---

<br>

## Final Thoughts

All three options are valid and useful.  
Choosing the right one improves:
- clarity
- safety
- long-term maintainability

If you are unsure:
- Web API → start with `IOptionsSnapshot`
- Background service → use `IOptionsMonitor`
- Security & infrastructure → `IOptions`

