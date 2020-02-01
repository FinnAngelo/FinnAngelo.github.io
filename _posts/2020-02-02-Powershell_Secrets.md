---
layout: post
title: "Powershell secrets so you don't commit them to github"
categories: powershell 
published: true
---
<figure style="float:right; margin-left:3em; width:50%;">
    <a href="http://zacharytotah.com/2016/09/pop-culture-tony-stark/gandalf-secrets-meme/">
        <img src="https://github.com/FinnAngelo/FinnAngelo.github.io/raw/master/_posts/images/Gandalf-Secrets-Meme.jpg" alt="A wizard always has secrets" />
    </a>
    <figcaption>Shhhh...</figcaption>
</figure>

It will come to a surprise to no one that commiting secrets to Github is a bad thing.

These methods here are using the Windows DPAPI which uses the Windows Indentity as keys to encrypt the secrets as files on the hard drive, so they can only be decrypted by the person logged in who created them.

And they should be stored well away from the github folder! 

```powershell
Set-StrictMode -Version Latest

function Set-Secret($secret, $filePath) {
    # https://blog.kloud.com.au/2016/04/21/using-saved-credentials-securely-in-powershell-scripts/
    $secureString = $secret | ConvertTo-SecureString -AsPlainText -Force 
    $secureStringText = $secureString | ConvertFrom-SecureString

    New-Item -Path $filePath -Type file -Force
    Set-Content -Path $filePath -Value $secureStringText
}

function Get-Secret($filePath) {  
    # https://stackoverflow.com/questions/28352141/convert-a-secure-string-to-plain-text
    $secureStringText = Get-Content $filePath
    $secureString = $secureStringText | ConvertTo-SecureString
    $secret = (New-Object PSCredential "user", $secureString).GetNetworkCredential().Password
    return $secret
}

Describe "Explore Setting Secrets for Powershell Modules" {

    Context "Check setting a secret" {
        It "When Set-Password, Then there is a new file" {
            #given
            $secret = "Howdy! I'm a secret!"
            $now = [System.DateTime]::Now.ToString("yyyy-MM-dd+HH-mm-ss-ffff")
            $filePath = "$env:LOCALAPPDATA\DpapiMe\Secret+$now.txt"

            #when
            Set-Secret $secret $filePath

            #then
            $result = Test-Path $filePath
            $result | Should Be True
        }
    }

    Context "Check getting a secret" {
        It "When Get-Password, Then the password comes from a file" {
            #given
            $now = [System.DateTime]::Now.ToString("yyyy-MM-dd+HH-mm-ss-ffff")        
            $secret = "Howdy! I'm a secret! I was set at $now!"
            $filePath = "$env:LOCALAPPDATA\DpapiMe\Secret+$now.txt"
            Set-Secret $secret $filePath

            #when
            $result = Get-Secret $filePath

            #then
            $result | Should Be $secret
        }
    }
}

```

## Credits ##

+ [Interworks - How to encrypt and store credentials securely](https://interworks.com/blog/trhymer/2013/07/08/powershell-how-encrypt-and-store-credentials-securely-use-automation-scripts/)
+ [Kloud - Using saved credentials in powershell](https://blog.kloud.com.au/2016/04/21/using-saved-credentials-securely-in-powershell-scripts/)
+ [Stackoverflow - Convert a secureString](https://stackoverflow.com/questions/28352141/convert-a-secure-string-to-plain-text)

If you find this useful, go be nice to someone. Pay it forward.

_Cheers!_
