﻿# AddGPLink

<a name="Add-GPLink"></a>
## Add-GPLink
### Synopsis
Links a new GPO to all OUs where a given GPO is already linked. Optionally removes the given GPO. Does not process GPOs linked to sites or the domain itself.
### Description
Sometimes, new GPOs need to be deployed everywhere a given GPO is already in use. Or a given GPO needs to be replaced globally after testing.

The Append-GPLink function takes a reference GPO and a new GPO (both must already exist). Then it enumerates the OUs where the reference GPO is linked. It then links the new GPO to these OUs (bottom most link order by default). Link order and link properties can be modified.

DYNAMIC PARAMETERS

-ReferenceGPO <String>
    The GPO that serves as a reference. The OUs this GPO is linked to are enumerated and updated.
    Tab completion searches the list of GPOs in TargetDomain.

    Required?                    true
    Position?                    2
    Default value                False
    Accept pipeline input?       false
    Accept wildcard characters?  false

-NewGPO <String>
    The GPO that will be linked to the OUs where ReferenceGPO is linked.
    This parameter is required if -RemoveLink is not specified.
    Tab completion searches the list of GPOs in TargetDomain.

    Required?                    true
    Position?                    3
    Default value                False
    Accept pipeline input?       false
    Accept wildcard characters?  false

### Syntax
```powershell
Add-GPLink [[-TargetDomain] <string>] -ReferenceGPO <string> -NewGPO <string> [-SearchBase <string>] [-OUFilter <string>] [-RegexEscape] [-RelativeLinkPos <string>] [-Enforced <EnforceLink>] [-LinkEnabled <EnableLink>] [-WhatIf] [-Confirm] [<CommonParameters>]

Add-GPLink [[-TargetDomain] <string>] -ReferenceGPO <string> -NewGPO <string> [-SearchBase <string>] [-OUFilter <string>] [-RegexEscape] [-ReplaceLink] [-Enforced <EnforceLink>] [-LinkEnabled <EnableLink>] [-WhatIf] [-Confirm] [<CommonParameters>]

Add-GPLink [[-TargetDomain] <string>] -ReferenceGPO <string> -NewGPO <string> [-SearchBase <string>] [-OUFilter <string>] [-RegexEscape] [-LinkOrder <int>] [-Enforced <EnforceLink>] [-LinkEnabled <EnableLink>] [-WhatIf] [-Confirm] [<CommonParameters>]

Add-GPLink [[-TargetDomain] <string>] -ReferenceGPO <string> [-SearchBase <string>] [-OUFilter <string>] [-RegexEscape] [-RemoveLink] [-WhatIf] [-Confirm] [<CommonParameters>]
```
### Parameters
#### TargetDomain &lt;String&gt;
    The domain where the actions should be performed. Defaults to the domain of the currently logged on user.
    
    Erforderlich?                false
    Position?                    1
    Standardwert                 $env:USERDNSDOMAIN
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### SearchBase &lt;String&gt;
    Use this distinguished name to limit the search for OUs where ReferenceGPO is linked to a specific searchbase. Since the domain is already defined, omit the domain from the searchbase (do not include the DC=... parts)
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### OUFilter &lt;String&gt;
    By default, all OUs are processed where the ReferenceGPO is linked. Use this parameter to restrict the OUs to process. The filter is evaluated as a regular expression match against the distinguished name of the OUs where the ReferenceGPO is linked.
    Filtering would be smarter if done via LDAPFilter, but there&#39;s no possibility to escape LDAP filters like it can be done with [Regex]::Escape for regular expressions.
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### RegexEscape [&lt;SwitchParameter&gt;]
    By default, the OUFilter will be used literally in a regex match. This means if you want to search for special characters like \ or *, you must escape them properly. Use this switch to let the cmdlet escape your filter string.
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 False
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### RelativeLinkPos &lt;String&gt;
    By default, NewGPO will be appended at the bottom of the linked GPOs. With RelativeLinkPos, you can specify whether NewGPO should be inserted directly above or below ReferenceGPO. Valid options are &quot;before&quot; and &quot;after&quot;.
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### ReplaceLink [&lt;SwitchParameter&gt;]
    Specify this switch if you want to remove the link to ReferenceGPO, leaving only the NewGPO link active. NewGPO will be linked at the position where ReferenceGPO was linked.
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 False
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### LinkOrder &lt;Int32&gt;
    By default, NewGPO is linked at the last position (bottom) or near ReferenceGPO. Specify a different LinkOrder to link it e.g. at the top (Linkorder 1) or anywhere in between.
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 0
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### Enforced
    Specify this parameter to select an enforcement state for the GPO link. The default is &quot;unspecified&quot; which effectively means &quot;not enforced&quot;. Valid options are &quot;unspecified&quot; (0), &quot;no&quot; (1) and &quot;yes&quot; (2).
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 Unspecified
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### LinkEnabled
    Specify this parameter to select an enablement state for the GPO link. The default is &quot;unspecified&quot; which effectively means &quot;enabled&quot;. Valid options are &quot;unspecified&quot; (0), &quot;no&quot; (1) and &quot;yes&quot; (2).
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 Unspecified
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### RemoveLink [&lt;SwitchParameter&gt;]
    Specify this switch to only remove the link to ReferenceGPO.
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 False
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### WhatIf [&lt;SwitchParameter&gt;]
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
#### Confirm [&lt;SwitchParameter&gt;]
    
    Erforderlich?                false
    Position?                    named
    Standardwert                 
    Pipelineeingaben akzeptieren?false
    Platzhalterzeichen akzeptieren?false
### Examples
#### BEISPIEL 1 
```powershell
Append-GPLink -ReferenceGPO 'Server Default Policy' -NewGPO 'Server Addon Policy'

```
Searches all OUs where the GPO named 'Server Default Policy' is linked, and links the GPO named 'Server Addon Policy' to these OUs. The link will be disabled and enforced.
#### BEISPIEL 2 
```powershell
Append-GPLink -ReferenceGPO 'Server Default Policy' -NewGPO 'Server New Default Policy' -OUFilter 'OU=Servers' -Replace -LinkOrder 1

```
Searches all OUs matching 'OU=Servers' where the GPO named 'Server Default Policy' is linked, and links the GPO named 'Server New Default Policy' to these OUs at position 1 . Then it removes the existing link to 'Server Default Policy'.
#### BEISPIEL 3 
```powershell
Append-GPLink -ReferenceGPO 'Server Default Policy' -Remove

```
Searches all OUs where the GPO named 'Server Default Policy' is linked, and removes the existing link.
<div style='font-size:small; color: #ccc'>Generated 13-10-2020 13:19</div>
