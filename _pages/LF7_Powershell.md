---
layout: page
title: Powershell
images: '/images/posts/Tux.png'
---

# Powershell installieren

Zum installieren von Powershell auf Deinem System besuche <https://github.com/PowerShell/PowerShell> und folge der Anweisung.

**Beachte** bitte folgende Datei: <https://github.com/PowerShell/PowerShell/blob/master/docs/KNOWNISSUES.md>!

# Warum Powershell

Powershell ist mit der Version 6 OpenSource Software geworden und unterstützt zahlreiche Plattformen wie Windows, iOS und alle gängige Linu-Plattformen (redhat, centos, Debian, Ubuntu etc.) und auch ARM-Prozessoren und Cloud-Anbieter wie Amazon, google und Azure von Microsoft. Der Vorteil liegt jetzt darin, dass man sich nicht mehr in einer hetrogenen Systemlanschaft auf jedes System einstellen muss, sondern alle Standardaufgaben in der gleichen Umgebung ausführen kann. Ebenfalls funktionieren geschriebene Skripte von einem Windowssystem auch auf allen anderen Systemen (wenn die Abhängigkeiten erfüllt sind).

# Starten der Powershell

Mit dem Befehl
````bash
pwsh
````

wird die Powershell gestartet. Mit dem Befehl

````bash
exit
````

verlässt man die Powershell wieder. 

Eine Übersicht der verfügbaren Befehle auf dem vorhandenen System bekommt man mit

````bash
get-command
````

Beim ausführen unter Ubuntu und Windows sieht man recht klar, welche Plattform am besten unterstützt wird.

# Beispiel

Um den Inhalt eines Directory anzuzeigen, verwendet Powershell den Befehl


````bash
get-childitem
````

Powershell versucht nun die verschiedenen Befehle der unterschiedlichen Plattformen zu vereinheitlichen. Bei Windows gibt es den Befehl

````bash
dir
````

und bei Linux hingegen

````bash
ls
````

Beide Befehle funktionieren auch auf allen Plattformen unter der Powershell. Aber der eigentliche Befehl heißt

````bash
get-childitem
````

# Hilfe

Die Powershell hat ein eingebautes Hilfesystem. Wenn wir uns beispielsweise alle Befehle anschauen möchten, die etwas mit den Prozessen auf unserem System zu tun haben:

````bash
> Get-Help *process*
                                                                                Name                              Category  Module                    Synopsis  ----                              --------  ------                    --------  Debug-Process                     Cmdlet    Microsoft.PowerShell.M... ...       Get-Process                       Cmdlet    Microsoft.PowerShell.M... ...       Start-Process                     Cmdlet    Microsoft.PowerShell.M... ...       Stop-Process                      Cmdlet    Microsoft.PowerShell.M... ...       Wait-Process                      Cmdlet    Microsoft.PowerShell.M... ...       Get-PSMetaConfigurationProcessed  Function  PSDesiredStateConfigur... ...       Set-PSMetaConfigDocInsProcesse... Function  PSDesiredStateConfigur... ... 
````

Wir suchen uns den Befehl Get-Process aus und möchten zu diesem Befehl mehr Informationen:

````bash
> get-help get-process                                                                                                                                  NAME                                                                                Get-Process                                                                 
SYNTAX
    Get-Process [[-Name] <string[]>] [-Module] [-FileVersionInfo]
    [<CommonParameters>]

    Get-Process [[-Name] <string[]>] -IncludeUserName  [<CommonParameters>]
````

Am Ende steht eventuell folgender Hinweis:

````bash
REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. It
    is displaying only partial help.
        -- To download and install Help files for the module that includes
    this cmdlet, use Update-Help.
        -- To view the Help topic for this cmdlet online, type: "Get-Help
    Get-Process -Online" or
           go to https://go.microsoft.com/fwlink/?LinkID=113324.

````


Dort ist einmal der Hinweis, dass die Hilfe nicht komplett auf unserem System installiert ist und wir dies mit dem Befehl

````bash
update-help -force
````

tun können. Jetzt wird die Hilfe wesentlich ausführlicher. Des weiteren können wir den Link öffnen der unten angegeben ist und die Hilfe online lesen. Auf einem graphischen System könnten wir eingeben:

````bash
get-help get-process -online
````

Die Hilfe ist noch umfangreicher als die installierte Hilfe.

# Die 4 Arten von Befehlen

Es gibt 4 Arten von Befehlen bei der Powershell:

1.) Administrative Befehle des Betriebssystems (ifconfig, ipconfig)

2.) Aliases wie dir und ls

3.) Skripte

4.) Cmdlets

## Alias

Alias sind Verweise oder Verlinkungen auf andere Befehle. Beispielsweise verweise ls und dir auf den Befehl Get-ChildItem. Eine Liste aller Aliases kann angezeigt werden mit:

````bash
Get-Alias
````
Wir bekommen eine Liste aller Alliases und das Ziel des Verweises angezeigt. Bitte beachte, dass bei Scripten auf Aliases verzichtet werden sollte, weil die Fehlersuche durch diese sehr erschwert wird.


## Cmdlets

Aussprache: Command Lets

Cmdlets sind eine neue Kategorie von Befehlen die es so bisher weder unter Windows noch Unix gab.  Sie sind daran zu erkennen, dass erst ein Verb kommt und dann ein Nomen. Wir schauen uns ein CMDlet an:

````bash
get-command
````

Wir bekommen eine Liste aller verfügbaren CMDlets in der Powershell angezeigt.

Cmdlets können von anderen Quellen installiert oder selbst programmiert werden. Sogenannte Snapins können sehr viele Cmdlets beinhalten (teilweise tausende). Verfügbar sind Snapins z.B. für VirtualBox, AWS, Azure etc.

Wir haben oben das Cmdlet Get-Process kennengelernt. Cmdlets können Parameter mit "-" angehängt werden, erfordern aber niemal zwingend einen Parameter. Wir möchten z.B. nur die Prozessinformationen über die offenen Shells wissen:

````bash
Get-Process -name bash
````

Probiere den Befehl auch mit "Bash" aus. Ist die Powershell Casesensitive? Beachtet die Linux-Konsole Groß- und Kleinschreibung?

# Verketten von Befehlen

Wir haben gesehen, dass wir mit

````bash
Get-Process -name bash
````

die aktiven Konsolen angezeigt bekommen. Jetzt möchten wie diese beenden:

````bash
Get-Process -name bash | Stop-Process
````

Unsere Sitzung wird beendet, weil wir den Prozess beendet haben. Das Zeichen 

````bash
|
````

nennt man "pipe" und bedeutet Röhre. Die Ausgabe vom ersten Befehl wird wie in einer Röhre nicht auf dem Bildschirm angezeigt sondern an dem nachfolgeneden Befehl weitergeleitet. Der Unterschied zur Linuxkonsole ist, dass dies ein Objekt ist nicht die einfache Textausgabe. Dies erhöht die Flexibilität ernorm:

````bash
Get-Process  | Select-Object -property CPU
````

Property bedeutet Eigenschaft und zeigt in diesem Fall vom Objekt Get-Process die Eigenschaft der CPU an. Um das ganze lesbarer zu gestalten lassen wir uns den Namen ausgeben:

````bash
Get-Process  | Select-Object -property name, CPU
````

Wir können auch mehere Pipes hintereinander fügen. Es soll jetzt beispielsweise die Prozesse nach der CPU-Belastung sortiert werden und nur die 10 Prozesse angezeigt werden, die die CPU am meisten belasten:

````bash
Get-Process | Select-Object -property name, CPU|Sort-Object -descending CPU|Select-Object -first 10
````

Um sinnvolle Konstrukte aufzubauen, brauchen wir eine Übersicht über die Einzelheiten des Objekts:

````bash
Get-Process | Get-Member
````

# Variable

Wir können das Objekt auch in einer Variable speichern. Eine Variable beginnt immer mit $:

````bash
$variable = Get-Process
````

Und auf die Variable, die das gesamte Objekt beinhaltet, den obigen Befehl anwenden;

````bash
$variable | Select-Object -property name, CPU|Sort-Object -descending CPU|Select-Object -first 10
````

# Module

Wir können auch Module bzw. SnapIns installieren, die den Befehlsvorrat der PowerShell-Installation deutlich erweitern:

````bash
Install-Package -Name AzureRM.NetCore
````
# Dateien

Um eine Ausgabe in eine Datei umzuleiten:

````bash
 Get-Process | Out-File -FilePath /home/user/process.csv
````

Bitte Pfad entsprechend anpassen. Die Ausgaber der Datei erfolgt mit:

````bash
cat process.csv
````

Eine Kodierung kann mit angegeben werden, ist aber sehr umständlich:

````bash
$encoding = New-Object System.Text.utf8encoding
Get-Process | Out-File -FilePath /home/user/process.csv -Encoding $encoding
````

Selbstverständlich könnte man den Befehl auch in eine Zeile schreiben. Die Verwendung der Variable dient nur dazu, zu verdeutlichen, welcher Teil die Kodierung vornimmt.

## Aufgabe

Entwickle eine Befehlszeile, die die aktuelle, lokale Netzwerkkonfiguration als .xml in eine Datei speichert (export-Clixml). Lass Dir die Datei auf dem Bildschirm erscheinen.

# Erstes Script

Wir legen eine Datei an mit dem Namen erst.ps mit folgendem Inhalt:

````bash
write-host "FBS ist cool"
````

Installoiert dafür testhalber das Programm Visual Studio Code, das eine integrierte PowerShell hat.

````bash
if(10 -gt 5)
{
write-host "Größer"
}
````

'-gt' steht für 'greater then' als 'größer als'. Eine Erklärung der wichtigsten Vergleichsopertoren gibt es hier: <https://www.windowspro.de/script/vergleichsoperatoren-powershell-eq-lt-gt-contains-match>.

Wir verändern den Code:

````bash
if(3 -gt 5)
{
write-host "Größer"
}
````

Jetzt solltze keine Ausgabe erfolgen!

Wir ergänzen den Code:

````bash
if(3 -gt 5)
{
    write-host "Größer"
}elseif(11 -gt 5)
{
    write-host "Juhu"
}
````

Das Skript wird wie erwartet ausgeführt. Setze jetzt Else
If in eine einzelne Zeile:

````bash
if(3 -gt 5)
{
    write-host "Größer"
}
elseif(11 -gt 5)
{
    write-host "Juhu"
}
````

Und Du bekommst eine Fehlermeldung. **Merke: ** Else oder ElseIf darf niemals aleine in einer Zeile stehen!

## Do - While

````bash
Do{
  $zahl = $zahl + 1
  write-host "$zahl x FBS ist cool"
  } while($zahl -lt 10)

````

## ForEach

Mit ForEach kann jedes Element eines Array ausgegeben werden:

````bash
$namen = "F","B","S","i"
ForEach($name in $namen){
write-host "$name"
}
````

# Powershell als Standardshell einrichten

Um Powershell als Standard einzurichten, sucht man sich selbst in der Datei /etc/passwd mit rinmem beliebigen Editor:

````bash
vim /etc/passwd
````

und verändert den Eintrag /bin/bash zu (nachdem man den Pfad überprüft hat):

````bash
user@joergreuter3:~$ whereis pwsh
pwsh: /usr/bin/pwsh
````

````bash
/usr/bin/pwsh
````

# Prozesse

Wir schauen uns nocheinmal den Befehl an um die laufende Prozesse angezeigt zu bekommen:

````bash
Get-Process
````

Eine sehr ungeordnete Ausgabe. Wir schränken die Ausgabe auf die ersten 1o Elemente ein:

````bash
Get-Process|Select-Object -first 10
````

Jetzt möchten wir die Prozesse absteigend nach CPU-Belastung sortieren:

````bash
Get-Process|Sort-Object -Descending CPU|Select-Object -first 10
````

Dann weißen wir das ganze einer Variable zu:

````bash
$belastung = Get-Process|Sort-Object -Descending CPU|Select-Object -first 10
````

Lassen uns die Variable anzeigen:

````bash
PS /home/user> $belastung

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
      0     0.00      17.87       4.51    1114 114 amazon-ssm-agen
      0     0.00       5.74       2.30       1   1 systemd
      0     0.00     102.17       2.00    1974 974 pwsh
      0     0.00       3.43       1.68    1098 098 iscsid
      0     0.00       6.79       0.49     380 380 systemd-journal
      0     0.00      25.59       0.39    1135 135 snapd
      0     0.00       0.14       0.35    1097 097 iscsid
      0     0.00      39.27       0.23    1147 147 node
      0     0.00       6.05       0.23    1139 139 accounts-daemon
      0     0.00       0.00       0.18       4   0 kworker/0:0
````

Jetzt möchten wir die Ausgabe als Json bekommen:

````bash
$belastung_json = Get-Process|Sort-Object -Descending CPU|Select-Object -first 10|ConvertTo-Json
```` 

Wir können auch die Variable direkt in Json umwandeln:

````bash
$belastung_json2 = $belastung| ConvertTo-Json
````

Wir lassen uns von der Prozessliste nur CPU und Name ausgeben:

````bash
Get-Process|Select-Object -property name, cpu
````

Und jetzt Name und virtueller Speicher (VM):

````bash
Get-Process|Select-Object -property name, vm
````

Wir möchten jetzt die Spalte VM nicht in Byte sondern in Gigabyte dargestellt bekommen.

````bash
PS /home/user> 1gb
1073741824
````

Wir müssen jetzt eine neue Spalte generieren die aus der Spalte VM den Wert nimmt und durch 1 GB teilt. Damit das Ergebniss leichter zu lesen ist, wandeln wir das Ergenis in ein Integer um:

````bash
Get-Process|Select-Object -property name, @{Label ='MGB';Expression ={$_.VM / 1gb -as [int]}}|Sort-Object -property MGB -Descending|Select-Object -first 10
````

Um die Berechnung der Spalte zu speichern, kann man folgenden Befehl verwenden:

````bas
Update-TypeData -Typename System.Diagnostics.process -MemberType ScriptProperty -MemberName MGB -value {$this.VM/1gb -as [int]}
````

Wir überprüfen das:

````bash
PS /home/user> Get-Process|Select-Object -property name, MGD                    
Name            MGD
----            ---
(sd-pam)          0
accounts-daemon   0
acpid             0
agetty            0
agetty            0
amazon-ssm-agen   0
````

## Aufgabe

Die Ausgabe soll absteigend nach MGB sortiert werden. Weiterhin sollen nur die ersten 10 Zeilen ausgegeben werden.

# Alias

Wir setzen das Alias meinProzess:

````bash
Set-Alias meinProzess Get-Process
meinProzess
````

und rufen das Alias in der nächsten Zeile auf.

## Aufgabe

Löse mit Alias folgende Aufgabe: Unter Windows soll unter dem Alias "myEditor" Notepad aufgerufen werdewn und unter Linux nano.

## Alias und Funktionen

Wir definieren eine Funktion CDOpt und rufen diese auf:

````bash
PS /home/user> function CDOpt {set-location /opt/}
PS /home/user> CDOpt
PS /opt>
````

Die Funktion bringt uns immer in ein bestimmtes Verzeichnis, egal an welchem Ort wir uns gerade befinden.

Wir können jetzt die Funktion mit einem Alias versehen:

````bash
PS /opt> Set-Alias my CDOpt
PS /opt> cd ~
PS /home/user> my
PS /opt>
````

Mit dem Befehl "my" rufen wir die Funktion CDOpt auf die uns in das Verzeichnis /opt bringt.

# Python in Powershell

Wir legen eine Datei an mit dem Namen fbs.py und folgendem Inhalt (vorher bitte ins Home-Verzeichnis wechseln mit dem Befehl "cd ~"):

````
#!/usr/bin/python
print("FBS ist cool!")
````

und setzen die Ausführungsechte und starten das Skript:

````bash
PS /home/user> chmod a+x fbs.py
PS /home/user> ./fbs.py
FBS ist cool!
````

# Arbeiten mit Dateien

Wir erstellen eine json-Datei (siehe oben) mit den ersten 10 Zeilen der Prozessliste, Der Befehl funktioniert nur unter Linux da head und cat Linux-Befehle sind:

````bash
PS /home/user> Get-Process|head|ConvertTo-Json|out-file ausgabe.json
PS /home/user> cat ./ausgabe.json
[
  "",
  " NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName                  ",
  " ------    -----      -----     ------      --  -- -----------                  ",
  "      0     0.00       2.16       0.00    2404 401 (sd-pam)                     ",
  "      0     0.00       6.18       0.56    1128 128 accounts-daemon              ",
  "      0     0.00       1.24       0.00    1146 146 acpid                        ",
  "      0     0.00       2.13       0.10    1259 259 agetty                       ",
  "      0     0.00       1.83       0.28    1264 264 agetty                       ",
  "      0     0.00      19.68       5.90    1137 137 amazon-ssm-agen              ",
  "      0     0.00       0.00       0.00      25   0 ata_sff                      "
]

````

Wir können den Inhalt wieder in PowerShell einlesen und als Variable speichern. Den Inhalt der Variable $Daten geben wir dann aus:

````bash
PS /home/user> $Daten = Get-Content ausgabe.json
PS /home/user> $Daten
[
  "",
  " NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName                  ",
  " ------    -----      -----     ------      --  -- -----------                  ",
  "      0     0.00       2.16       0.00    2404 401 (sd-pam)                     ",
  "      0     0.00       6.18       0.56    1128 128 accounts-daemon              ",
  "      0     0.00       1.24       0.00    1146 146 acpid                        ",
  "      0     0.00       2.13       0.10    1259 259 agetty                       ",
  "      0     0.00       1.83       0.28    1264 264 agetty                       ",
  "      0     0.00      19.68       5.90    1137 137 amazon-ssm-agen              ",
  "      0     0.00       0.00       0.00      25   0 ata_sff                      "
]

````

Wir können das Json-File auch konvertieren in die Ursprügliche Ausgabe:

````bash
PS /home/user> $Daten = Get-Content ausgabe.json|ConvertFrom-Json
PS /home/user> $Daten

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
      0     0.00       2.16       0.00    2404 401 (sd-pam)
      0     0.00       6.18       0.56    1128 128 accounts-daemon
      0     0.00       1.24       0.00    1146 146 acpid
      0     0.00       2.13       0.10    1259 259 agetty
      0     0.00       1.83       0.28    1264 264 agetty
      0     0.00      19.68       5.90    1137 137 amazon-ssm-agen
      0     0.00       0.00       0.00      25   0 ata_sff
````

Aber nicht nur das, dass gesamte Objekt steht wieder zur Verfügung!:

````bash
PS /home/user> $Daten|Sort-Object -Descending CPU

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
      0     0.00       2.16       0.00    2404 401 (sd-pam)
      0     0.00       6.18       0.56    1128 128 accounts-daemon
      0     0.00       1.24       0.00    1146 146 acpid
      0     0.00       2.13       0.10    1259 259 agetty
      0     0.00       1.83       0.28    1264 264 agetty
      0     0.00      19.68       5.90    1137 137 amazon-ssm-agen
      0     0.00       0.00       0.00      25   0 ata_sff
````

Und wir können uns alle Member des Objekts anzeigen lassen:

````bash
$Daten|Get-Member
````






