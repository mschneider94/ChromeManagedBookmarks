# Chrome Managed Bookmarks

Simplify administration of _**Managed Bookmarks**_ for _**Google Chrome**_ using YAML

## Main Idea

The Managed Bookmarks-Feature of Google Chrome requires JSON-Input.

Working with large lists of managed bookmarks directly in JSON can easily get complicated...  
...especially when you're deploying those via GPO, as those take the whole JSON-String in one line.

YAML-files are very readable and easy to work in a text editor.  
So why not maintain the list of managed bookmarks inside a YAML-File and just convert the YAML into JSON to update the GPOs?

## How To

If you haven't already, [set Chrome Browser policies on managed PCs](https://support.google.com/chrome/a/answer/187202) and create a [Managed Bookmarks GPO](https://admx.help/?Category=Chrome&Policy=Google.Policies.Chrome::ManagedBookmarks)...

Migrate your managed bookmarks into a YAML-File (like [Example.yaml](Example.yaml)), you could use a Converter for that, too.

Whenever you need to edit yout managed bookmarks, just edit those inside that YAML-File.  
Afterwards you'll just convert the YAML into JSON and overwrite the JSON inside the GPO with that.

### YAML to JSON Conversion

Just use any one of the various YAML-JSON-Converters on the internet...  
...or use the PowerShell-Module [powershell-yaml](https://github.com/cloudbase/powershell-yaml), which you can also isntall from the [PowerShell Gallery](https://www.powershellgallery.com/) using `Install-Module powershell-yaml`.

If you store your YAML-file inside a Git-Repo (on GitHub, GitLab, Gitea, ...), you could build yourself a One-Liner for PowerShell to get and convert the YAML-file in one command!

```
# PowerShell

Import-Module powershell-yaml

ConvertFrom-Yaml $(Invoke-WebRequest -URI "https://raw.githubusercontent.com/mschneider94/ChromeManagedBookmarks/main/Example.yaml").Content | ConvertTo-Yaml -JsonCompatible
```

## Automation

If you store your YAML-file inside a Git-Repo using your own (local) Instance of [Gitea](gitea.io), have a look at the [Node-RED flow](Node-RED.json) I've created to illustrate that possibility.

In there you'll just swap out the _"placeholder for anything to really update bookmarks"_ Node with something to actually update the GPO (there PowerShell-Nodes for Node-RED)...  
...create a webhook in your Gitea-Repository for _Push_-Events to that Node-RED flow...
...and the GPO will be automatically updated, when the YAML-file is changed.
