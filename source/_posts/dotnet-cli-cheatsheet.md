---
title: .NET CLI cheatsheet
date: 2025-12-24 23:28:03
tags: [.net, cli, terminal, cheatsheet]
category: [".net"]

thumbnail: uploads/dotnet-cli-cheatsheet/cover.png
banner: uploads/dotnet-cli-cheatsheet/cover.png
---
![Alt Text](uploads/dotnet-cli-cheatsheet/cover.png)

This guide explains the **most important `dotnet` commands**, grouped logically.
You do **not** need to memorize everything — learn the structure and look up details when needed.

<!--more-->

<br>


## Mental Model

Think of `dotnet` as a **Swiss Army knife** with 5 main categories:

1. SDK & environment info  
2. Project creation & structure  
3. Build & run (development)  
4. Publish & distribution  
5. Tools & advanced commands  

<br>

## SDK & Environment Information

### `dotnet --info`
Shows:
- Installed SDKs
- Installed runtimes
- OS & architecture

```bash
dotnet --info
```

---

### `dotnet --version`
Shows the currently active SDK version.

```bash
dotnet --version
```

---

### `dotnet --list-sdks`
Lists all installed SDKs.

```bash
dotnet --list-sdks
```

---

### `dotnet --list-runtimes`
Lists all installed runtimes.

```bash
dotnet --list-runtimes
```

<br>

## Project Creation

### `dotnet new`
Lists available templates.

```bash
dotnet new
```

---

### Common templates

```bash
dotnet new console     # Console application
dotnet new webapi      # ASP.NET Core Web API
dotnet new classlib    # Class Library
dotnet new sln         # Solution file
```

---

### Create project with name

```bash
dotnet new console -n MyApp
```

<br>

## Solution & Dependencies

### `dotnet sln add`
Adds a project to a solution.

```bash
dotnet sln add MyApp/MyApp.csproj
```

---

### `dotnet add package`
Installs a NuGet package.

```bash
dotnet add package Newtonsoft.Json
```

---

### `dotnet remove package`
Removes a NuGet package.

```bash
dotnet remove package Newtonsoft.Json
```

---

### `dotnet restore`
Restores NuGet dependencies.

```bash
dotnet restore
```

> Usually runs automatically.


<br>

## Build & Run (Development)

### `dotnet build`
Compiles the project (does not run).

```bash
dotnet build
```

---

### `dotnet run`
Builds and runs the project.

```bash
dotnet run
```

---

### `dotnet clean`
Deletes `bin/` and `obj/` folders.

```bash
dotnet clean
```

<br>

## Testing

### `dotnet test`
Runs unit tests.

```bash
dotnet test
```

<br>

## Publishing & Distribution (Very Important)

### `dotnet publish`
Creates a distributable output.

```bash
dotnet publish -c Release
```

---

### Self-contained (no .NET required on target PC)

```bash
dotnet publish -c Release -r win-x64 --self-contained true
```

✔ Runs on any Windows x64 machine  
❌ Larger output size  

---

### Single-file executable

```bash
dotnet publish -c Release -r win-x64 \
  --self-contained true \
  /p:PublishSingleFile=true
```

✔ One `.exe` file  
✔ Easy to distribute  

---

### Portable folder (recommended for stability)

```bash
dotnet publish -c Release -r win-x64 --self-contained true
```

✔ No installer  
✔ Fewer runtime issues  


<br>


## Runtime Identifiers (RID)

| Platform | RID |
|--------|-----|
| Windows x64 | win-x64 |
| Windows x86 | win-x86 |
| Linux x64 | linux-x64 |
| macOS Intel | osx-x64 |
| macOS Apple Silicon | osx-arm64 |


<br>

## Global Tools

### Install global tool

```bash
dotnet tool install -g dotnet-ef
```

---

### List global tools

```bash
dotnet tool list -g
```

---

### Update global tools

```bash
dotnet tool update -g dotnet-ef
```

<br>


## Entity Framework Core (Quick Overview)

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<br>


## What You Should Memorize

- `dotnet new`
- `dotnet run`
- `dotnet build`
- `dotnet publish`
- `--self-contained`
- `-r win-x64`

Everything else is **reference knowledge**.

<br>


## Final Advice

ASP.NET and .NET are designed to be **modular and discoverable**.

Learn the structure.  
Look up the details.  
Build real projects.
