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

- Code Style Preferences: https://learn.microsoft.com/en-us/visualstudio/ide/code-styles-and-code-cleanup?view=vs-2022
- Code Style Rules (full example): https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/code-style-rule-options

### Good to know: how to apply all .editorconfig settings on save of file:

Tools, Options, Text-Editor, Code-Cleanup:

1. "Select Code Cleanup Profile on Save profile": select "Profile 1" 
2. Enable "[x] run Code Cleanup Profile on Save"
3. click "Configure Code Cleanup": Add Option "Fix all warnings and errors set in EditorConfig" to the top

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
More secure version:

```ps
# $targetDirectory = "C:\Path\To\Root\Directory"
$targetDirectory = "."

# Function to check if "*.csproj" file is present in the folder
function CheckFileType {
    param (
        [string]$type,
        [string]$path
    )

    # Check if "*.csproj" file is present in the folder
    if (Test-Path "$path\$type") {
        return $true
    }

    return $false
}

# Recursive function to delete "bin" and "obj" folders
function DeleteBinObjFolders {
    param (
        [string]$path
    )

    # Get "bin" and "obj" folders within the current directory
    $binFolders = Get-ChildItem -Path $path -Filter "bin" -Directory -Recurse
    $objFolders = Get-ChildItem -Path $path -Filter "obj" -Directory -Recurse
    $pckFolders = Get-ChildItem -Path $path -Filter "packages" -Directory -Recurse
    $vsFolders = Get-ChildItem -Path $path -Filter ".vs" -Directory -Recurse
    $nodeFolders = Get-ChildItem -Path $path -Filter "node_modules" -Directory -Recurse
    $distFolders = Get-ChildItem -Path $path -Filter "dist" -Directory -Recurse
    $angularFolders = Get-ChildItem -Path $path -Filter ".angular" -Directory -Recurse
    $nxFolders = Get-ChildItem -Path $path -Filter ".nx" -Directory -Recurse
    $vscodeFolders = Get-ChildItem -Path $path -Filter ".vscode" -Directory -Recurse

    # Delete "bin" and "obj" folders if "*.csproj" file is present above
    if (CheckFileType -type "*.csproj" -path $path) {
        $binFolders | Remove-Item -Recurse -Force
        $objFolders | Remove-Item -Recurse -Force
        $pckFolders | Remove-Item -Recurse -Force
        # $vsFolders | Remove-Item -Recurse -Force
        Write-Host "Deleted 'bin', 'obj', 'packages', '.vs' folders in: $path near .csproj"
    }
    else {
        # Write-Host "Skipping deletion as no '*.csproj' file found above: $path"
    }

    if (CheckFileType -type "*.sln" -path $path) {
        $pckFolders | Remove-Item -Recurse -Force
        $vsFolders | Remove-Item -Recurse -Force
        Write-Host "Deleted 'package', '.vs' folders in: $path near .sln"
    }
    else {
        # Write-Host "Skipping deletion as no '*.sln' file found above: $path"
    }

    if (CheckFileType -type "package.json" -path $path) {
        $nodeFolders | Remove-Item -Recurse -Force
        $distFolders | Remove-Item -Recurse -Force
        $angularFolders | Remove-Item -Recurse -Force
        $nxFolders | Remove-Item -Recurse -Force
        # $vscodeFolders | Remove-Item -Recurse -Force
        Write-Host "Deleted 'node_modules', 'dist', '.angular', '.nx', '.vscode' folders in: $path near package.json"
    }
    else {
        # Write-Host "Skipping deletion as no 'package.json' file found above: $path"
    }
}

# Recursively delete folders recursively
Get-ChildItem -Path $targetDirectory -Directory -Recurse | ForEach-Object {
    DeleteBinObjFolders -path $_.FullName
}
```

## Use Visual Studio as `difftool` and `mergetool`

- If you haven't, install `everything` in your windows machine (best search tool ever: <https://www.voidtools.com/>)
- Search for `vsdiffmerge` to get its path
- Found it here: `C:\Program Files\Microsoft Visual Studio\2022\Enterprise\Common7\IDE\CommonExtensions\Microsoft\TeamFoundation\Team Explorer\vsDiffMerge.exe`
- Open git-extensions, open settings for `difftool` and `mergetool`
- Select `vsdiffmerge` and use the `vsdiffmerge` path
