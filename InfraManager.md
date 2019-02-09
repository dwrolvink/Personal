# InfraManager voor beginners
## Introductie
InfraManager is een module (of set van meerdere modules) in powershell geschreven om het beheer van de Rijkswaterstaat infrastructuur te vereenvoudigen. En dan in het bijzonder de werkzaamheden van IRI-Windows.

Goede kans als je dit leest dat je al het fijne is uitgelegd over dit pakket. Voor meer informatie zie hier voor een volledige documentatie van dit pakket.

## Logging
### LogFilePath
Het gehele pakket maakt gebruik van `Write-Log` om logging weg te schrijven. De locatie van het log bestand wordt in `$script:LogFilePath` weggeschreven. Hier verwijst $script naar de scope van de logging module, en kan dus niet direct opgevraagd worden, dit doe je daarentegen als volgt:

```powershell
Get-LogFilePath
```
Dit zal je niks teruggeven omdat we de LogFilePath nog niet ingesteld hebben. Laten we dat meteen doen:

```powershell
Set-LogFilePath
```
Zoals je kunt zien kiest `Set-LogFilePath` zelf een locatie uit. Als de G:/ schijf niet beschikbaar is slaat het de logging per default in jouw documenten folder weg (`Documents\Logs`). Je krijgt dan deze melding: `WARNING: Could not create G:\Inframanager\, saving in C:\Users\dorus\Documents\Logs instead`. 

Daarnaast kun je zien dat Set-LogFilePath de locatie van het logbestand teruggeeft. De naamgeving: `2019-02-09.22.02.51.csv` is simpelweg de timestamp (creeer je eigen met `Get-Timestamp`!) + .csv. 

Dit is een ';' separated log file. Dit teken is dan ook niet toegestaan in de logging berichten.

Verdere opties voor Set-LogFilePath zijn:

```powershell
# Verander alleen de naam van het bestand
Set-LogFilePath -FileName "MijnEigenLogNaam.csv" 

# Verander alleen de folder waar de logging wordt opgeslagen
Set-LogFilePath -Folder "D:/Logging" 
```

Set-LogFilePath maakt zelf de folder aan als je daar rechten toe hebt. Let er op dat elk script dat Write-Log, Set-LogFilePath, of Get-LogFilePath gebruikt, naar hetzelfde log wegschrijft. Als je powershell afsluit en opnieuw opstart is het logpath weer leeg. 

Als je logging wilt scheiden per script, bijvoorbeeld, je schrijft een deployment script voor SQL, zet dan in het begin van je script:

```powershell
Set-LogFilePath -FileName ("DeploymentSQL_" + Get-Timestamp + ".csv")
```

En, om te voorkomen dat andere scripts, die deze regel niet ingesteld hebben staan, naar hetzelfde script wegschrijven, kun je dit op het einde zetten:

```powershell
Clear-LogFilePath
```

### Write-Log
Zoals eerder al duidelijk is geworden kiest de logging module automatisch een logfilepath uit. `Write-Log` kijkt of `Get-LogFilePath` een waarde teruggeeft, en zo niet dan roept deze `Set-LogFilePath` aan (zonder parameters). Deze gaat dan op zijn beurt kijken op welke locatie een logbestand weggeschreven moet worden, en test ook gelijk of het bestand schrijfbaar is.

Hieronder vindt je een aantal verschillende opties:

```powershell
# Schrijf een standaard bericht weg
Write-Log "Script has started" 

# Hetzelfde als hierboven, maar laat niks in de console zien
Write-Log "Script has started" | Out-null

# Schrijf een error weg in het log
Write-Log "Big oof" -Type "Error" | Out-Null

# Laat Write-Log een daadwerkelijke error schrijven
Write-Log "Big oof" -Type "Error" -ForcePrint | Out-Null
```

`Write-Log` gebruikt  `Write-Error` om de error in de console te tonen. Als je een andere waarde geeft voor `-Type` dan zal `Write-Host` gebruikt worden. `-Type "Warning"` gebruikt `Write-Host $Message -Foregroundcolor Yellow`. Als `-ForcePrint` niet wordt meegegeven dan wordt het bericht door `Write-Verbose` geleid, en zal je deze logging alleen zien in de console als je dit aanzet.

> By default, Windows PowerShell is set up to use Write-Verbose. The issue is that out of the box, verbose messages do not display. The $VerbosePreference preference variable controls if Write-Verbose statements appear. By default, the value of $VerbosePreference is set to SilentlyContinue, which means the messages do not appear. (...) If I want to change it, the statement is this: $VerbosePreference = "continue"

Het specifieke gedrag van `Write-Log` zal nog wel veranderen (waarschijnlijk minder vaak dan deze handleiding!), dus neem zelf een kijkje in de `Logger.psm1` als je de "waarheid" van alle mogelijke instellingen wilt weten. 

## Testing
De "nested module" InternalTesting voorziet het pakket van Unit/Mock testing. Het is "best practice" om voor elke functie/cmdlet die dit redelijkerwijs ondersteund ieg basic unit testing in te richten. CRUD functies (Create, Read, Update, Delete) wil je vaak niet as-is testen, hiervoor kun je "mock testing" gebruiken. Dit is allemaal buiten de scope van deze handleiding, maar kijk ieg naar "Pester" hoe je dit voor Powershell inricht.

Wat wel binnen de scope is, is hoe je de bestaande tests aanroept. `Invoke-Tests` roept alle tests 1 voor 1 aan. Een goede test om mee te beginnen is `Test-LoggerModule`. De standaardnaamgeving is altijd "Test-[Naam van de Module]Module".

Als je in `Logger.test.ps1` kijkt (voor mijn machine op `C:\Program Files\WindowsPowerShell\Modules\InfraManager\Modules\InternalTests\Functions\Logger.test.ps1`) zie je hetvolgende:

``` powershell
Function Test-LoggerModule()
{
    Describe "Set-LogFilePath"{
        It "sets/returns path to a writeable file (no parameters)"{
            $path = Set-LogFilePath
            "test" > $path
            Get-Content $path | Should Be "test"
            remove-item $path
        }

        It "sets/returns path to a writeable file (Filename set)"{
            $path = Set-LogFilePath -FileName "test.test"
            "test" > $path
            Get-Content $path | Should Be "test"
            remove-item $path
        }

        It "sets/returns path to a writeable file (Filename set, Folder set)"{
            $path = Set-LogFilePath -FileName "test.test" -Folder ($env:USERPROFILE + "\Documents\")
            "test" > $path
            Get-Content $path | Should Be "test"
            remove-item $path
        }
        It "returns false if it can't create a writeable file (Filename set, Folder set wrongly)"{
            $path = Set-LogFilePath -FileName "test.test" -Folder ("V:/")
            $path | Should Be $false
        }
        It "doesn't change the logfilepath if an invalid path is given"{
            $OriginalPath = Set-LogFilePath
            Set-LogFilePath -Folder "V:/nonvalid/filepath" | Out-Null
            Get-LogFilePath | Should Be $OriginalPath
        }
    }
    
    (...)
}

```

Hier zie je hopelijk een beetje hoe de structuur van de tests in elkaar steekt. Wederom, zie ["Pester"](https://github.com/pester/Pester/wiki/Describe).

## CredentialManager
Om niet meer te hoeven bouwen op password files, maakt InfraManager gebruik van CredentialManager. Deze slaat de wachtwoorden op in het gelijknamige systeem van Windows. 

CredentialManager (windows) slaat Credentials op op basis van "Target", dit is typisch een URL, maar het kan elke waarde zijn. Gezien het feit dat er per target maar 1 credential opgeslagen kan worden, moet onze module creatief zijn. We doen dit door de username en url te combineren tot `username@url`

Om ons te houden aan de URL standaard is er gekozen voor de volgende opbouw: `[protocol]://[username]@[host]`. Dit kan bijvoorbeeld zijn: `http://svc@intranet.nl`, maar in ons geval zullen we ons vooral houden aan de `rws://username@applicatienaam.dc15` opbouw, (of dc10 voor systemen in de oude omgeving). Uiteindelijk maakt het weinig uit, als we maar dezelfde target opvragen als dat we opslaan. Laten we zien hoe dit in zijn werk gaat:

``` powershell
# Sla nieuwe credential op
New-StoredCredential -Username "serviceaccount1" -URL "rws://applicatie.dc15" -Password "eitje"
```

Als je dit uitvoert zie je dat deze functie een waarde teruggeeft: `rws://serviceaccount1@applicatie.dc15` dit is de volledige target!

Nu we aan het testen zijn willen we niet allemaal vervuiling krijgen in de CredentialManager, laten we dus meteen deze test credential weer verwijderen:

``` powershell
# Gebruik de handige target:
Remove-StoredCredential -Target "rws://serviceaccount1@applicatie.dc15"

# Dit mag ook:
Remove-StoredCredential -URL "rws://applicatie.dc15" -UserName "serviceaccount1"
```
Let erop dat `Remove-StoredCredential` nooit wat terug geeft. Let er daarnaast op dat `New-StoredCredential` niet checkt of de target al bestaat, maar er vrolijk overheen wegschrijft.

Gezien de credentials op de werkstation worden opgeslagen, en niet een netwerk locatie, moeten we in de code rekening houden dat de credential wellicht niet aanwezig is. Hieronder een stukje voorbeeld code om dit dicht te bouwen:

``` powershell
    # Get credential
    $Credential = Get-StoredCredential -Username $ApiUserName -URL $ApiCredentialURL

    If (!$Credential){
        $Password = Read-Host "Please insert the password for $($ApiUserName). You should only have to do this once per server (per account)." 
        New-StoredCredential -Username $ApiUserName -URL $ApiCredentialURL -Password $Password | Out-Null
    }
```

In de toekomst komt er wellicht een script om alle benodigde wachtwoorden in 1 keer in te stellen op een nieuwe management server. Gezien je dan weer in passwordfile gebied komt moet hier nog even over nagedacht worden.
