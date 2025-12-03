---
title: "IIS vs Kestrel: What's the Difference?"
date: 2025-02-16 20:02:10
tags: ["ASP.NET Core", "IIS", "Kestrel", "Web Servers"]
category: ["web servers"]

thumbnail: uploads/iis-vs-kestrel-diff/cover.png
banner: uploads/iis-vs-kestrel-diff/cover.png
---
![Alt Text](uploads/iis-vs-kestrel-diff/cover.png)

# IIS vs Kestrel: What is the Difference?

When you create an ASP.NET Core web app, it runs on a web server. You might have heard about **Kestrel** and **IIS** — but what’s the difference between them? Let’s keep it simple.

<!--more-->

## Kestrel

**Kestrel** is a fast, lightweight, and cross‑platform web server built into ASP.NET Core.  
It can run on **Windows**, **Linux**, and **macOS** — and it’s the default server for all new ASP.NET Core apps.

### Key points
- Works on any platform — great for Docker and cloud hosting.  
- Very fast and efficient, supports HTTP/1.1, HTTP/2, and HTTP/3.  
- Configured in your app (code or `appsettings.json`).  
- Doesn’t include modules like URL rewrite, compression, or authentication — you add them in code.  
- Usually placed **behind a reverse proxy** like IIS, Nginx, or Apache in production for extra security and management.

Example setup in `Program.cs`:
```csharp
builder.WebHost.ConfigureKestrel(options =>
{
    options.Limits.MaxRequestBodySize = 50 * 1024 * 1024; // 50 MB
    options.Limits.KeepAliveTimeout = TimeSpan.FromSeconds(120);
    options.AddServerHeader = false;
});
```

## IIS

**IIS (Internet Information Services)** is a mature, feature‑rich web server built into Windows.  
It can also host ASP.NET Core apps — either **in‑process** or **out‑of‑process** (through Kestrel).

### Key points
- Windows only — integrated with the OS.  
- Supports **App Pools**, automatic restarts, health checks, and detailed logging.  
- Built‑in modules for URL Rewrite, caching, compression, authentication, and more.  
- Centralized TLS/SSL certificate management.  
- Can share ports (80/443) between multiple apps.

When hosting ASP.NET Core on IIS:
- **In‑process**: the app runs directly inside IIS (`w3wp.exe`) — fastest option.  
- **Out‑of‑process**: IIS acts as a reverse proxy to Kestrel.

## Which one should you use?

| Scenario | Recommended Server |
|-----------|-------------------|
| Development or local testing | Kestrel |
| Docker or Linux deployment | Kestrel |
| Windows enterprise environment | IIS |
| Need Active Directory (Windows Auth) | IIS |
| Cloud (Azure, AWS, etc.) with load balancer | Kestrel + reverse proxy |

## Integration tip

When running behind a proxy (like IIS or Nginx), remember to use the **Forwarded Headers Middleware** in your app:

```csharp
app.UseForwardedHeaders();
```

This ensures your app knows the correct client IPs and HTTPS status.

## Summary

- **Kestrel** is the default server — fast, modern, and cross‑platform.  
- **IIS** is Windows‑only but offers rich management, security, and enterprise‑grade features.  
- Many production apps use **both**: Kestrel to run the app, IIS (or Nginx) as a reverse proxy in front of it.

