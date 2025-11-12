# PowerShell Cheat Sheet

## Basic Commands

### Navigation and File System

```powershell
# Get current location
Get-Location          # or pwd

# Change directory
Set-Location C:\Path  # or cd C:\Path

# List files and folders
Get-ChildItem         # or ls, dir

# List with details
Get-ChildItem -Force  # Include hidden files

# Recursive listing
Get-ChildItem -Recurse
```

### File Operations

```powershell
# Create new file
New-Item file.txt -ItemType File

# Create new directory
New-Item folder -ItemType Directory

# Copy file
Copy-Item source.txt destination.txt

# Move file
Move-Item file.txt C:\NewLocation\

# Remove file
Remove-Item file.txt

# Remove directory (recursive)
Remove-Item folder -Recurse -Force

# Get file content
Get-Content file.txt

# Set file content
Set-Content file.txt "New content"

# Append to file
Add-Content file.txt "Appended text"
```

## Variables and Data Types

```powershell
# Define variables
$name = "John"
$age = 30
$items = @(1, 2, 3, 4, 5)
$hash = @{Name="John"; Age=30}

# String interpolation
$message = "Hello, $name"

# Get variable type
$age.GetType()

# Arrays
$array = @("item1", "item2", "item3")
$array[0]           # Access element
$array.Count        # Get length
```

## Conditionals and Loops

### If-Else

```powershell
if ($age -gt 18) {
    Write-Host "Adult"
} elseif ($age -eq 18) {
    Write-Host "Just turned adult"
} else {
    Write-Host "Minor"
}
```

### Loops

```powershell
# ForEach loop
foreach ($item in $items) {
    Write-Host $item
}

# For loop
for ($i = 0; $i -lt 10; $i++) {
    Write-Host $i
}

# While loop
while ($count -lt 10) {
    Write-Host $count
    $count++
}
```

## Comparison Operators

```powershell
-eq     # Equal
-ne     # Not equal
-gt     # Greater than
-ge     # Greater than or equal
-lt     # Less than
-le     # Less than or equal
-like   # Wildcard comparison
-match  # Regular expression match
```

## Pipeline and Filtering

```powershell
# Pipeline
Get-Process | Where-Object {$_.CPU -gt 100}

# Select specific properties
Get-Process | Select-Object Name, CPU

# Sort results
Get-Process | Sort-Object CPU -Descending

# Filter and format
Get-Service | Where-Object {$_.Status -eq "Running"} | Format-Table Name, Status
```

## Functions

```powershell
function Get-Greeting {
    param(
        [string]$Name
    )
    return "Hello, $Name!"
}

# Call function
Get-Greeting -Name "John"

# Function with default parameter
function Add-Numbers {
    param(
        [int]$a = 0,
        [int]$b = 0
    )
    return $a + $b
}
```

## Working with Objects

```powershell
# Create custom object
$person = [PSCustomObject]@{
    Name = "John"
    Age = 30
    City = "New York"
}

# Access properties
$person.Name

# Get object members
$person | Get-Member
```

## Common Cmdlets

```powershell
# Get help
Get-Help Get-Process
Get-Help Get-Process -Examples

# Get command information
Get-Command *service*

# Measure execution time
Measure-Command { Get-Process }

# Export to CSV
Get-Process | Export-Csv processes.csv -NoTypeInformation

# Import from CSV
Import-Csv processes.csv

# Convert to JSON
Get-Process | Select-Object Name, ID | ConvertTo-Json

# Write output
Write-Host "Message"      # To console
Write-Output "Data"       # To pipeline
Write-Error "Error!"      # Error message
Write-Warning "Warning!"  # Warning message
```

## Process Management

```powershell
# Get processes
Get-Process

# Get specific process
Get-Process -Name "notepad"

# Stop process
Stop-Process -Name "notepad"
Stop-Process -Id 1234

# Start process
Start-Process notepad.exe
```

## Service Management

```powershell
# Get all services
Get-Service

# Get specific service
Get-Service -Name "wuauserv"

# Start service
Start-Service -Name "wuauserv"

# Stop service
Stop-Service -Name "wuauserv"

# Restart service
Restart-Service -Name "wuauserv"
```

## Remote Management

```powershell
# Execute command on remote computer
Invoke-Command -ComputerName Server01 -ScriptBlock {
    Get-Process
}

# Start remote session
Enter-PSSession -ComputerName Server01

# Exit remote session
Exit-PSSession

# Run script on remote computer
Invoke-Command -ComputerName Server01 -FilePath C:\Scripts\script.ps1
```

## Error Handling

```powershell
try {
    # Code that might throw an error
    Get-Item "C:\NonExistent.txt" -ErrorAction Stop
}
catch {
    Write-Host "An error occurred: $_"
}
finally {
    Write-Host "Cleanup code"
}
```

## String Operations

```powershell
# Concatenation
$fullName = $firstName + " " + $lastName

# String methods
$text = "Hello World"
$text.ToUpper()
$text.ToLower()
$text.Replace("World", "PowerShell")
$text.Split(" ")
$text.Substring(0, 5)
$text.Length

# Test if string contains
$text -like "*World*"
$text -match "World"
```

## Regular Expressions

```powershell
# Match pattern
"test123" -match "\d+"
$matches[0]  # Access matched value

# Replace with regex
"test123" -replace "\d+", "456"

# Split with regex
"one,two;three" -split "[,;]"
```

## Module Management

```powershell
# List installed modules
Get-Module -ListAvailable

# Import module
Import-Module ModuleName

# Get module commands
Get-Command -Module ModuleName

# Install module from PowerShell Gallery
Install-Module -Name ModuleName

# Update module
Update-Module -Name ModuleName
```

## Execution Policy

```powershell
# Get current execution policy
Get-ExecutionPolicy

# Set execution policy
Set-ExecutionPolicy RemoteSigned

# Bypass execution policy for single script
PowerShell.exe -ExecutionPolicy Bypass -File script.ps1
```

## Useful Tips

```powershell
# Clear screen
Clear-Host  # or cls

# Get command history
Get-History

# Run previous command
Invoke-History 1

# Alias management
Get-Alias
Set-Alias -Name ll -Value Get-ChildItem
```
