---
layout: post
title: "Powershell secrets, so you don't commit them to GitHub"
categories: powershell 
published: true
---
<figure style="float:right; margin-left:3em; width:50%;">
    <a href="http://zacharytotah.com/2016/09/pop-culture-tony-stark/gandalf-secrets-meme/">
        <img src="https://github.com/FinnAngelo/FinnAngelo.github.io/raw/master/_posts/images/Gandalf-Secrets-Meme.jpg" alt="A wizard always has secrets" />
    </a>
    <figcaption>Shhhh...</figcaption>
</figure>

It will not surprise anyone that commiting secrets to GitHub is a bad thing.

I have spent minutes looking on the googles, and I haven't found the exact equivalent of [the dotnet user secrets](https://docs.microsoft.com/en-us/aspnet/core/security/app-secret) for powershell.

When developing my dirty goofball [this-will-never-get-committed](https://securitytrails.com/blog/github-dorks) scripts, I want zero-risk for my secrets; logins to azure, connection strings for databases, ~~how I like hawiian pizza~~, and anything I don't want the world to know.

----------------------------------------

## TOC ##

+ [Add CurrentUserAllHosts profile](#Add-CurrentUserAllHosts-profile)
+ [Set-MyDevSecret and Get-MyDevSecret](#Set-MyDevSecret-and-Get-MyDevSecret)
+ [Using the MyDevSecret functions](#Using-the-MyDevSecret-functions)
+ [Credits](#Credits)

----------------------------------------

## Add CurrentUserAllHosts profile ##

I'm intending to use this for my Azure CLI learnin', so I want this secrets stuff available to me all the time.

This is in my [Powershell Snippets](http://www.finnangelo.com/2019/05/24/Powershell_Snippets.html#Persistent-profile) article, but here it is again...

```powershell
if (!(Test-Path $Profile.CurrentUserAllHosts)) {
    New-Item -Type file -Path $Profile.CurrentUserAllHosts -Force }
```

----------------------------------------

## Set-MyDevSecret and Get-MyDevSecret ## 

These functions are using the Windows DPAPI.  
This uses the Windows Indentity as keys (etc) to encrypt the secrets as files on the hard drive, so they can only be decrypted by the person logged in who created them.

And as the secrets should be stored well away from the github folder, I put the encrypted files on some rando non-home drive.

On my PC this is at `D:\Users\Jon\Documents\WindowsPowerShell\profile.ps1`

```powershell
Set-StrictMode -Version Latest

# ---------------------------------------------------
# User Secrets
# http://www.finnangelo.com/powershell/2020/02/02/Powershell_Secrets.html

function Set-MyDevSecret([String] $key, [String] $secret) {
    $filePath = "G:\MyDevSecret\$key.txt"
    $secureString = $secret | ConvertTo-SecureString -AsPlainText -Force 
    $secureStringText = $secureString | ConvertFrom-SecureString

    New-Item -Path $filePath -Type file -Force
    Set-Content -Path $filePath -Value $secureStringText
}

function Set-MyDevSecretFromSecureString([String] $key, [secureString] $secureSecret) {
    $filePath = "G:\MyDevSecret\$key.txt"

    $secureStringText = $secureSecret | ConvertFrom-SecureString

    New-Item -Path $filePath -Type file -Force
    Set-Content -Path $filePath -Value $secureStringText
}

function Get-MyDevSecret($key) {  
    $filePath = "G:\MyDevSecret\$key.txt"
    $secureStringText = Get-Content $filePath
    $secureString = $secureStringText | ConvertTo-SecureString
    $secret = (New-Object PSCredential "user", $secureString).GetNetworkCredential().Password
    return $secret
}
```

----------------------------------------

## Using the MyDevSecret functions ##

```powershell
Describe "Explore Setting Secrets for Powershell Modules" {

    Context "Check setting a secret" {
        It "When Set-MyDevSecret, Then there is a new file" {
            #given
            $secret = "Howdy! I'm a secret!"
            $key = [System.DateTime]::Now.ToString("yyyy-MM-dd+HH-mm-ss-ffff")

            #when
            Set-MyDevSecret $key $secret

            #then
            $result = Test-Path "G:\MyDevSecret\$key.txt"
            $result | Should Be True
        }
    }

    Context "Check getting a secret" {
        It "When Get-MyDevSecret, Then the secret comes from a file" {
            #given
            $key = [System.DateTime]::Now.ToString("yyyy-MM-dd+HH-mm-ss-ffff")        
            $secret = "Howdy! I'm a secret! I was set at $key!"
            Set-MyDevSecret $key $secret

            #when
            $result = Get-MyDevSecret $key

            #then
            $result | Should Be $secret
        }
    }
}
```

And here is our pester tests passing:

<img src="https://github.com/FinnAngelo/FinnAngelo.github.io/raw/master/_posts/images/MyTestsForPowershellSecrets.png" alt="Running 'Explore Setting Secrets for Powershell Modules' with success" />

----------------------------------------

## Credits ##

+ [Interworks - How to encrypt and store credentials securely](https://interworks.com/blog/trhymer/2013/07/08/powershell-how-encrypt-and-store-credentials-securely-use-automation-scripts/)
+ [Kloud - Using saved credentials in powershell](https://blog.kloud.com.au/2016/04/21/using-saved-credentials-securely-in-powershell-scripts/)
+ [Stackoverflow - Convert a secureString](https://stackoverflow.com/questions/28352141/convert-a-secure-string-to-plain-text)

If you find this useful, go be nice to someone. Pay it forward.

_Cheers!_
