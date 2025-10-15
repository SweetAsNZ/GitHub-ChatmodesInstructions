---
applyTo: '**.ps1,**.psm1'
description: 'General frontend coding guidelines for PowerShell and Copilot'
---

## Core Development Rules
- **Never add parameter aliases** unless explicitly listed in user request or called function's definition.
- **Never** - return generated output in chat if file is open. Always edit files directly.
- **Always** Offer changelog entries after making changes to cmdlets/scripts.
- **PSScriptAnalyzer exceptions** - never rename functions, create rule exceptions below [CmdletBinding()].


## Parameter Management Guidelines
 
### Core Parameter Rules

- **Never** add aliases unless explicitly listed in the user request or called function's parameter definition. 
- **Do not**  add any aliases for new parameters unless the alias is present in the user request or in the called function's definition.
- **Never remove parameters** unless explicitly requested by the user - only adjust aliases to resolve conflicts.
- **Always** preserve original parameters during edits - each parameter must remain separate with its own purpose.
- **Match** called function aliases exactly for pass-through parameters.
 
### Parameter Editing Safety Protocol
**Pre-Edit**: Read entire parameter section, verify all parameters will remain intact.
**During Edit**: One parameter at a time, include 3-5 lines context, maintain separator integrity.

# Project general coding guidelines

## Naming Conventions
- **Always** Use clear and descriptive names for all variables, functions, and classes.
- **Always** Follow a consistent naming scheme (e.g., camelCase for variables and functions, PascalCase for classes).
- **Always** Avoid aliases, abbreviations and acronyms unless they are widely understood.
- **Always** Include context in names to make their purpose clear (e.g., `userList` instead of `list`).
- **Always** Follow PSScriptAnalyzer guidelines for naming conventions.
- **Always** Avoid using reserved words or keywords in names.
- **Always** Avoid using existing command names or Function names to prevent confusion.
- **Always** Include units in variable names where applicable (e.g., `fileSizeInBytes`).
- **Always** Avoid using special characters or spaces in names.
- **Always** The script name should be the same as the function name unless multiple functions are embedded in a script in which case it should be the first function name.
- **Always** Use approved Verbs for function names (e.g., `Get`, `Set`, `Remove`...).
- **Always** Include a noun to indicate the resource being acted upon (e.g., `Get-User`, `Set-Configuration`...).

## Parameter Names:
- Use PascalCase
- **Always** Choose clear, descriptive names.
- **Always** Use singular form unless always multiple.
- **Always** Follow PowerShell standard names.

## Variable Names:
- **Always** Use PascalCase for public variables.
- **Always** Use camelCase for private variables.
- **Always** Avoid abbreviations.
- **Always** Use meaningful names.

## Alias Avoidance:
- **Always** Use full cmdlet names.
- **Always** Avoid using aliases in scripts (e.g., use Get-ChildItem instead of gci).
- **Always** Document any custom aliases.
- **Always** Use full parameter names.

# Pipeline and Output
## Pipeline Input:
- **Always** Use ValueFromPipeline for direct object input.
- **Always** Use ValueFromPipelineByPropertyName for property mapping.
- In scripts exceeding 1000 lines Implement Begin/Process/End blocks for pipeline handling.
- **Always** Document pipeline input requirements.

# Output Objects:
- **Always** Return rich objects, not formatted text.
- **Always** Use PSCustomObject for structured data.
- **Always** Enable downstream cmdlet processing.

# Pipeline Streaming:
- Output one object at a time.
- Use process block for streaming.
- Avoid collecting large arrays.
- **Always** Enable immediate processing.

# PassThru Pattern:
- Default to output for action cmdlets.
- **Always** Implement -PassThru switch for object return.
- **Always** Return modified/created object with -PassThru.
- **Always** Use verbose/warning for status updates.

## Parameter Design
# Standard Parameters:
- Use common parameter names (Path, Name, Force).
- **Always** Follow built-in cmdlet conventions.
- Use aliases for specialized terms.
- **Always** Document parameter purpose.

# Parameter Names:
- **Always** Use singular form unless always multiple.
- **Always** Choose clear, descriptive names.
- **Always** Follow PowerShell conventions.
- Use PascalCase formatting.

# Type Selection:
- **Always** Use common .NET types.
- **Always** Implement proper validation.
- Consider ValidateSet for limited options.
- **Always** Enable tab completion where possible.

# Switch Parameters:
- Use [switch] for boolean flags.
- Avoid $true/$false parameters.
- Default to $false when omitted.
- **Always** Use clear action names.

## Error Handling and Safety
# ShouldProcess Implementation:
- Use [CmdletBinding(SupportsShouldProcess = $true)].
- **Always** Set appropriate ConfirmImpact level.
- Call $PSCmdlet.ShouldProcess() for system changes.
- Use ShouldContinue() for additional confirmations.

# Message Streams:
- Write-Verbose for operational details with -Verbose.
- Write-Warning for warning conditions.
- Write-Error for non-terminating errors.
- throw for terminating errors.
- Avoid Write-Host except for user interface text.

# Error Handling Pattern:
- **Always** Use try/catch blocks for error management.
- **Always** Set appropriate ErrorAction preferences.
- **Always** Return meaningful error messages.
- **Always** Use ErrorVariable when needed.
- **Always** Include proper terminating vs non-terminating error handling.
- **Always** In advanced functions with [CmdletBinding()], prefer $PSCmdlet.WriteError() over Write-Error.
- **Always** In advanced functions with [CmdletBinding()], prefer $PSCmdlet.ThrowTerminatingError() over throw.
- **Always** Construct proper ErrorRecord objects with category, target, and exception details.

# Non-Interactive Design:
- **Always** Accept input via parameters.
- **Always** Support automation scenarios.
- **Always** Document all required inputs.

## Documentation and Style
# Comment-Based Help: 
- **Always** Include comment-based help for any function or cmdlet.

### Good Example - Comment Based Help
.SYNOPSIS Brief description
.DESCRIPTION Detailed explanation
.PARAMETER descriptions
.EXAMPLE sections with practical usage
.NOTES Additional information

# Consistent Formatting:
- **Always** Follow consistent PowerShell style.
- **Always** Use proper indentation (4 spaces recommended).
- **Always** Opening braces on same line as statement.
- **Always** Closing braces on new line.
- **Always** Use line breaks after pipeline operators.
- **Always** PascalCase for function and parameter names.
- Avoid unnecessary whitespace.

# Pipeline Support:
- **Always** Implement Begin/Process/End blocks for pipeline functions in scripts over 1000 lines.
- Use ValueFromPipeline where appropriate.
- Support pipeline input by property name.
- Return proper objects, not formatted text.

# Avoid Aliases: 
- **Always** Use full cmdlet names and parameters.
- **Always** Avoid using aliases in scripts (e.g., use Get-ChildItem instead of gci); aliases are acceptable for interactive shell use.
- **Always** Use Where-Object instead of ? or where.
- **Always** Use ForEach-Object instead of %.
- **Always** Use Get-ChildItem instead of ls or dir.

## Code Quality
- **Always** Check for security vulnerabilities and follow best practices for secure coding.
- **Always** Ensure code is efficient and optimized for performance.
- **Always** Preface the Context Sensitive Help (CSH) comment block with the phrase "# WIP" until told it is working.
- **Always** Use meaningful variable and function names that clearly describe their purpose.
- **Always** Include comments to explain the purpose and logic of your code except for very basic lines.
- **Always** Include helpful comments for complex logic.
- Add error handling for user inputs and API calls.
- Verify commands work with PowerShell 5.1, unless otherwise specified and make sure to add a comment if PS 5.1 is not supported well commented.
- Verify commands are valid for the versions of applications being targeted.
- **Always** Include tests to validate the behavior of your code.
- Ensure code is modular and reusable.
- **Always** Follow PSScriptAnalyzer guidelines for code quality e.g. $null on the left of an expression.
- **Always** Assume I want to use this code in a production environment, so it must be reliable and well-tested.
- Assume **Always** I want to use functions from this code in other scripts, so they must be well-documented and reusable.
- **Always** Use functions rather than just a simple script except for one-liner commands.
- **Always** Check $Profile for user-specific configurations and preferences.
- **Always** Include logging for important events and errors using the existing logger function.
- **Always** Ensure sensitive information is not logged.
- **Always** Use structured logging where possible using the function logger from $profile.
- **Always** Include correlation IDs for tracking requests across services.
- Include user IDs or other relevant metadata in logs.
- **Always** Use "Write-Host -f Green" for successful events. Append " - Success" to the message.
- **Always** Use "Write-Host -f Red" for error events. Append " - FAILED" to the message. 
- **Always** Use "Write-Host -f Yellow" for warning events. Append " - Warning" to the message.
- **Always** Include $Err = $PSItem.Exception.Message ; $Err on the line before the error. Include $Err in the error output and log.
- **Never** use $Host or any built-in PowerShell commands as a variable  in scripts to avoid errors.
- **Always** Include a description of the script's purpose and functionality in the header comments.
- **Always** Include a SYNOPSIS section in the header comments.
- **Always** Include a PARAMETERS section in the header comments.
- **Always** Include an EXAMPLES section in the header comments.
- **Always** Include a NOTES section in the header comments. With Author:, Company:, Date:, Version:. 
- **Always** Insert or update the current date and version in the NOTES section.
- **Always** Include a CHANGELOG section in the header comments.
- **Never** Include Variables that are NOT used.
- **Always** Include a CHANGELOG section in the header comments.
- **Always** Include a TODO section in the header comments.
- **Always** Include any known issues or limitations in the TODO section.
- **Always** Include a list of future enhancements or improvements in the TODO section.
- **Always** Include a section for tracking changes and updates to the script in the CHANGELOG section.
- **Always** Export-Csv must be followed by -NoTypeInformation
- **Always** Verify All instructions herein are followed
- **Always** Verify All instructions in the user request are followed
- **Always** Use region and endregion to organize code into logical sections rather than comments above script blocks
- **Always** Name the endregion the same as the region it closes
- **Always** Include a region for Output and Logging if the script produces output or logs information

# GOOD EXAMPLE HEADER NOTES COMMENTS
.NOTES
    Author: Tim West
    Company: Sweet As Chocolate/Air New Zealand
    Date: 28/08/25
    Version: 1.0
# END GOOD EXAMPLE HEADER NOTES COMMENTS

## Memory Properties (Win32_OperatingSystem)
- Use `TotalVisibleMemorySize` (not TotalPhysicalMemory) for total RAM.
- Values in KB - divide by 1024² for GB: `(Get-CimInstance Win32_OperatingSystem).TotalVisibleMemorySize / 1024 / 1024`
- Include error handling and unit validation.

