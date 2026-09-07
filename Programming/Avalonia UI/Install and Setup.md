Looks like there are two ways to use Avalonia UI:
1. Visual Studio - the official Visual Studio extension is commercial only
2. VS Code - the VS Code extension is open-source

Thinking the move is to use the VS Code Extension then


## Start with these Commands

Before creating a VS Code workspace, just create a project with these commands.

Install Avalonia UI:
```
dotnet new install Avalonia.Templates
```

Create project folder inside current directory:
```
dotnet new avalonia.mvvm -o MyApp
```

Or in VS Code you can do:
```
Ctrl  + Shift + P
.NET: New Project
Avalonia MVVM App
```

### This is a lot like WPF
I am noticing that this is structured a lot like WPF, which is good as this is what I am used to.