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
- [x] Debugging, Symbols: enable symbol file (.pdb) locations. I set the path to c:\temp\symbols\

Details: <https://devblogs.microsoft.com/devops/understanding-symbol-files-and-visual-studios-symbol-settings/>

## Performance

- Tools: Options: Disable all XAML options, if you don't need it
- Text Editor: All Languages: Code Lense: disable it (also helps to reduce distraction, but unittest status might be helpful)

## Force update

```ps
Start-Process -Wait -FilePath  "C:\Program Files (x86)\Microsoft Visual Studio\Installer\vs_installer.exe" -ArgumentList "update --passive --norestart --installpath ""C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise"""
```

## ReSharper

ReSharper is very useful and I recommend it. But there are some downsides like cost and impacts on performance (especially on solution load time).
There are other tools which are free and might help on your daily work. Check out my ReSharper alternatives in "Visual Studio Extensions"

## Settings

See chapter .editorconfig for setting all the configuration:

- to avoid git issues, make sure all team members agree on the same tabs/spaces/indentation settings
- reduce merge conflicts by [x] Place 'System' directories first when sorting usings

## Visual Studio Extensions (ReSharper alternatives)

Currently, I have all the following extensions installed and I cannot see any performance issues.

### Why us tools

I like this option for several reasons:

- Automatically clean up your code, to avoid unused code (sort usings to avoid merge conflicts, remove unused usings)
- Format your code: this avoids merge conflicts. It is especially important, when you work in a team.
- Check the VS Code tools as well: <https://github.com/boeschenstein/vscode1_information>

### CodeMaid

An extension to cleanup and simplify our C#. *Format on save* is not enabled by default: Please enable this option: "Automatically run cleanup on save file".

### Roslynator

A collection of 500+ analyzers, refactorings and fixes for C#.

### SonarLint

Detect Code Quality and Security issues on the fly.

### UnitTest Boilerplate Generator

Generates a unit test boilerplate from a given C# class, setting up mocks for all dependencies. This tool can save you a lot of time crafting Unit tests. It supports all common frameworks. (XUnit, Moq, ...)

### Visual Studio Team (Mads Kristensen)

- https://github.com/madskristensen/BasicEssentials
    - Add New File
    - Comment Remover
    - Developer News
    - File Icons
    - Font Sizer
    - Insert GUID
    - Markdown Editor
    - Output Enhancer
    - Theme Switcher
    - Tweaks

- VS 2019: https://github.com/madskristensen/WebEssentials2019
- VS 2022: https://marketplace.visualstudio.com/items?itemName=MadsKristensen.TheEssentials2022

### Libman - Microsoft Library Manager

Handle clientside packages in Visual Studio (similar to npm, webpack, yarn, bower)

- <https://github.com/aspnet/LibraryManager>

### Code Coverage (TODO)

https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-code-coverage?tabs=windows

TODO: evaluate free tools:

- https://dotnetfoundation.org/projects/coverlet
- https://github.com/OpenCover/opencover
- https://doc.froglogic.com/squish-coco/latest/coveragescanner.html
- https://github.com/SteveGilham/altcover

Reporting

- https://github.com/danielpalme/ReportGenerator

### Comparing single files

- https://marketplace.visualstudio.com/items?itemName=MadsKristensen.FileDiffer
- https://www.meziantou.net/comparing-files-using-visual-studio.htm

### Dependency Governance

NsDepCop

- http://www.plainionist.net/Dependency-Governance-DotNet/
- https://github.com/realvizu/NsDepCop

## Solution Cleanup

### Reset all changes

>This cannot be undone!

This resets all changese in the source folder to the current branch.

'git clean -fdx'

### Solution Cleanup Script

```powershell
# to run this: 
# 1) close visual Studio first
# 2) right click this file, run in PowerShell

$file1ToCheck = ".\MyVeryCoolSolution.sln"
"Current directory: " + (Get-Item -Path ".\").FullName
if ((Test-Path $file1ToCheck -PathType leaf))
{
    "Solution file found."
} else {
    throw "Solution file not found in current directory"
}

# delete subfolders

'========================== BEFORE ========================== '

Get-ChildItem .\ -include packages,bin,obj,bld,Backup,_UpgradeReport_Files,Debug,Release,ipch,help,.vs -Recurse 

'========================== DELETING NOW ==================== '

Get-ChildItem .\ -include packages,bin,obj,bld,Backup,_UpgradeReport_Files,Debug,Release,ipch,help,.vs -Recurse | foreach ($_) { remove-item $_.fullname -Force -Recurse }

'========================== AFTER =========================== '

Get-ChildItem .\ -include packages,bin,obj,bld,Backup,_UpgradeReport_Files,Debug,Release,ipch,help,.vs -Recurse 

```

## Use Visual Studio as `difftool` and `mergetool`

- If you haven't, install `everything` in your windows machine (best search tool ever: <https://www.voidtools.com/>)
- Search for `vsdiffmerge` to get its path
- Found it here: `C:\Program Files\Microsoft Visual Studio\2022\Enterprise\Common7\IDE\CommonExtensions\Microsoft\TeamFoundation\Team Explorer\vsDiffMerge.exe`
- Open git-extensions, open settings for `difftool` and `mergetool`
- Select `vsdiffmerge` and use the `vsdiffmerge` path
