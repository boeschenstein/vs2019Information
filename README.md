# Visual Studio 2019 - Infos and Tooling

## Debug into source code

<https://docs.microsoft.com/en-us/visualstudio/ide/go-to-and-peek-definition>

## .editorconfig

```config
[*.cs]

indent_style = space
indent_size = 4
insert_final_newline = true

# Organize usings
dotnet_sort_system_directives_first = true

# RCS1090: Call 'ConfigureAwait(false)'.
dotnet_diagnostic.RCS1090.severity = none
```

## Source code of .NET Core

<https://source.dot.net/>

Classic .NET: <https://referencesource.microsoft.com/>

## Stepping into source code

Be aware of all security and performance issues!

Tools: Options:

- [ ] Enable Just My Code
- [x] Enable .NET framework source stepping
- [x] Debugging, Symbols: enable symbol file (.pdb) locations 

Details: <https://devblogs.microsoft.com/devops/understanding-symbol-files-and-visual-studios-symbol-settings/>

## Performance

Tools: Options: Disable all XAML options, if you don't need it

## ReSharper

ReSharper is very useful and I recommend it. But there are some downsides like cost and impacts on performance (especially on solution load time).
There are other tools which are free and might help on your daily work. Check out my ReSharper alternatives in "Visual Studio Extensions"

## Settings

see chapter .editorconfig for setting all the configuration:

- to avoid git issues, make sure all team members agree on the same tabs/spaces/indentation settings
- reduce merge conflicts by [x] Place 'System' directories first when sorting usings

## Visual Studio Extensions (ReSharper alternatives)

I currently have all the following extensions installed and I cannot see any performance issues.

### CodeMaid

An extionsion to cleanup and simplify our C#. I really like this option, which is not enabled by default: "Automatically run cleanup on save file".

### Roslynator

A collection of 500+ analyzers, refactorings and fixes for C#

### SonarLint

Detect Code Quality and Security issues on the fly

### UnitTest Boilerplate Generator

Generates a unit test boilerplate from a given C# class, setting up mocks for all dependencies.
