---
title: .NET – Launch Settings (Basics)
date: 2024-10-20 19:10:45
tags: [.NET, launchSettings, Development]
category: [".NET"]

thumbnail: uploads/dotnet-launch-settings/cover.png
banner: uploads/dotnet-launch-settings/cover.png
---
![Alt Text](uploads/dotnet-launch-settings/cover.png)

## What is the `launchSettings.json` file?

- **Location:** `Properties\launchSettings.json`  
- **Purpose:** defines *local run profiles* — URLs, HTTPS, environment variables, and startup behavior.  
- **Important:** used only during local development (`dotnet run`, Visual Studio); **does not affect production**.

<!--more-->

---

## Key Fields

- **`profiles`** – the collection of profiles (e.g., `http`, `https`).  
- **`commandName`** – `Project` → runs the app directly with Kestrel (not IIS Express).  
- **`applicationUrl`** – the URLs the app listens to (separated by `;`).  
- **`environmentVariables`** – environment variables (e.g., `ASPNETCORE_ENVIRONMENT`).  
- **`launchBrowser`** – automatically opens the browser at startup (true/false).  
- **`dotnetRunMessages`** – displays active URL messages in the console.

---

## Your Profiles

Example minimal file:

```json
{
  "profiles": {
    "http": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "http://localhost:5062",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "https": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "https://localhost:7165;http://localhost:5062",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

- `http` – listens only on `http://localhost:5062`.  
- `https` – listens on both `https://localhost:7165` and `http://localhost:5062`.  
- If you have `app.UseHttpsRedirection()` in `Program.cs`, the `http` profile will automatically redirect to HTTPS.  
  👉 For normal development, use the **`https` profile**.

---

## Link to the Code

The variable `ASPNETCORE_ENVIRONMENT = Development` activates conditional blocks such as:

```csharp
if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
}
```

---

## How to Run with a Profile

CLI:
```bash
dotnet run --launch-profile https
```

Visual Studio / Rider / VS Code:  
select the desired profile from the run dropdown.

---

## Practical Tips

- Want local HTTPS? Install the development certificate:
  ```bash
  dotnet dev-certs https --trust
  ```
- Changing ports? Edit `applicationUrl` and update your `.http` or frontend config.  
- You can add custom variables:
  ```json
  "environmentVariables": {
    "ASPNETCORE_ENVIRONMENT": "Development",
    "ConnectionStrings__Default": "Host=...;Database=...;"
  }
  ```

---

## Summary

- **What it is:** a launch profile configuration file used only in development (`Properties\launchSettings.json`).  
- **Used by:** `dotnet run`, Visual Studio, Rider, VS Code.  
- **What it configures:** URLs (`applicationUrl`), environment variables (`ASPNETCORE_ENVIRONMENT`), startup options (`launchBrowser`).  
- **What it doesn’t do:** does not affect build/publish and is not used in production.  
  For production → use `appsettings*.json`, environment variables, and host configuration.

---

**In short:**  
`launchSettings.json` is a local development tool that defines *how* your .NET app runs on your machine — not *where* or *how* it will run in production.
