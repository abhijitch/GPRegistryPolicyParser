# Registry Policy Parser Cmdlets

**Future updates to this project will be implemented in the [GPRegistryModule](https://github.com/PowerShell/GPRegistryPolicy).**
Version 0.2 will continue to be available in this GitHub project
and in the [PowerShellGallery](https://www.powershellgallery.com/packages/GPRegistryPolicyParser/0.2).
This will ensure any projects taking a dependency on GPRegistryParser 0.2
will not be broken.
Any updates to GPRegistryParser will be documented in the GPRegistryModule
release notes,
but none are expected.

These cmdlets will allow you to work with .POL files,
which contain the registry keys enacted by Group Policy.
The primary intent of these cmdlets is to
enable enforcing security policy settings on Nano Server,
but this method will also work on Windows Server 2016.
These cmdlets are used internally by *GPRegistryModule*.

---

## Parse-PolFile
Reads a .pol file containing group policy registry entries
and returns an array of objects each containing a registry setting.

### Syntax
```
Parse-PolFile [-Path <string>]  [<CommonParameters>]
```

| Parameter Name | Description                                                                            | 
| ---            | ---                                                                                    |
| Path           | Specifies the path to the .pol file to be imported.                                    |

### Example
```
C:\PS> $RegistrySettings = Parse-PolFile -Path "C:\Registry.pol"
```

---

## Read-RegistryPolicies
Reads given registry entries and returns an array of registry settings.

### Syntax
```
Read-RegistryPolicies [-Division <string>] [-Entries <string[]>]  [<CommonParameters>]
```

| Parameter Name | Description                                                                                          | 
| ---            | ---                                                                                                  |
| Division       | Specifies the target registry division (LocalMachine, CurrentUser or Users)                          |
| Entries        | Specifies the list of registry keys to be exported. The default value is set to 'Software\Policies'. |

### Example
```
C:\PS> $RegistrySettings = Read-RegistryPolicies -Entries @('Software\Policies\Microsoft\Windows', 'Software\Policies\Microsoft\WindowsFirewall')

C:\PS> $RegistrySettings = Read-RegistryPolicies -Division 'CurrentUser'

C:\PS> $RegistrySettings = Read-RegistryPolicies -Division 'LocalMachine' -Entries @('Software\Policies\Microsoft\Windows', 'Software\Policies\Microsoft\WindowsFirewall')
```

---

## Create-RegistrySettingsEntry
Creates a .pol file entry byte array from a GPRegistryPolicy instance.
This entry can be written in a .pol file later.

### Syntax
```
$RegistrySettings = Create-RegistrySettingsEntry [-RegistryPolicy <GPRegistryPolicy[]>
```

| Parameter Name | Description                                                                                          | 
| ---            | ---                                                                                                  |
| RegistryPolicy | An instance of internal type 'GPRegistryPolicy'                                                      |

### Example
```
C:\PS> $Entry = Create-RegistrySettingsEntry -RegistryPolicy $GPRegistryPolicyInstance
```

---

## Append-RegistryPolicies
Appends an array of registry policy entries to a file.
The file must already have a valid header.

### Syntax
```
Append-RegistryPolicies [-RegistryPolicies <GPRegistryPolicy[]>] [-Path <string>]
```

| Parameter Name   | Description                                                                                          | 
| ---              | ---                                                                                                  |
| RegistryPolicies | An array of instance of internal type 'GPRegistryPolicy'                                             |
| Path             | Specifies the path to the .pol file to be imported.                                                  |

### Example
```
C:\PS> Append-RegistryPolicies -RegistryPolicies $RegistryPoliciesInput -Path "C:\Registry.pol"
```

---
