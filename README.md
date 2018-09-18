# workspaceaufsetzen

echo "# workspaceaufsetzen" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/ghstcoder47/workspaceaufsetzen.git
git push -u origin master


== Git-Client konfigurieren

 git config --global core.autocrlf true
 git config --global push.default matching
 git config --global branch.autosetuprebase always 
 git config --global http.proxy http://proxy-k.innovas.de:3128

    Werte ersetzen:

 git config --global user.name "Vorname Nachname"
 git config --global user.email <xxx>@innovas.de

    Optinal

 REM Optinal: anderer Editor als den großartigen vim, wieso sollte man sowas wollen, aber nagut, der Vollständigkeit halber
 git config --global core.editor „'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession –noPlugin“
 REM Optinal: nur den aktuellen branch pushen
 git config --global push.default current


== Beispiel
Zu Beginn der tatsächlichen Arbeiten auf dem refactoring-Branch ist dann noch ein Entwicklerbranch zu erstellen:

git checkout -b entwickler/mbn/req359

git push -u origin entwickler/mbn/req359 

== info

Featureentwicklung
Definition Feature

Ein Feature ist entweder eine durch einen Defect, ein Requirement oder ein Arbeitspaket angestossene Softwareanpassung.
Beginn der Featureentwicklung (1)

Codeanpassungen im Rahmen der Featureumsetzung prägen sich in 2 Arten von Tätigkeiten aus:

    Die zur Fehlerbehebung oder zur Requirementumsetzung benötigten Anpassungen (Namensschema: feature/[Name des Releases]/[req oder def][Nr des Req oder Defects])
    Refactoringmaßnahmen, die im Rahmen der Featureentwicklung durchgeführt werden und zur Verbesserung der Codequalität beitragen. (Namensschema: refactoring/[Name des Releases]/[req oder def][Nr des Req oder Defects])

Die nach obigem Schema erstellten Branches werden gepusht und bleiben im Remote-Repository bestehen, solange die Entwicklung läuft.

Von release/[Name des Releases] kann kein branch gezogen werde. Hier muss der Branch nach dem folgenden Schema gezogen werden(Identisch zu punkt 1): (Namensschema: feature/[Name des Releases]/[req oder def][Nr des Req oder Defects])
Arbeit auf dem Feature-/Refactoring-Branch (2)

Die tatsächlichen Anpassungen werden dann auf Entwickler-Branches durchgeführt, die dem folgenden Namensschema entsprechen: entwickler/[Kürzel des jeweiligen Entwicklers]/[req oder def][Nr des Req oder Defects]

Diese Entwickler-Branches dürfen auch aus Backupgründen ins Remote-Repository gepusht werden. Allerdings müssen sie dort, nach dem Merge in den Feature-Branch, wieder entfernt werden.
Beispiel

Zu Beginn der tatsächlichen Arbeiten auf dem refactoring-Branch ist dann noch ein Entwicklerbranch zu erstellen:

git checkout -b entwickler/mbn/req359

git push -u origin entwickler/mbn/req359
Abschluss der Arbeiten auf Branches (3)

Nachdem die beabsichtigten Änderungen auf dem Entwicklerbranch vorgenommen wurden, sind diese in den Feature- oder Refactoringbranch zurück zu mergen. Nach Merge aller an einem Feature beteiligten Entwicklerbranches wird ein Review auf die getätigten Änderungen auf dem Feature- oder Refactoringbranch vollzogen. Wurde hier keine Änderungsnotwendigkeit festgestellt, darf der Branchinhalt in den master gemerged werden.

Die Merges in den master werden durch die technischen Verantwortlichen mbn/plu durchgeführt!

Für den Merge Entwickler -> Feature und Feature -> Master kommen 2 verschiedene Vorgehensweisen in Frage:

    Fast Forward only

        Source- und Target-Branch pullen ( git checkout <source>; git pull; git checkout <target>; git pull)
        Auf den Source-Branch wechseln (git checkout <source>)
        Rebase mit dem Target-Branch (git rebase <target>)
        Kompilieren (mvn clean install)
        Auf den Target-Branch wechseln (git checkout <target>)
        Source-Branch mit Fast Forward Only mergen (git merge --ff-only <source>)
        Source-Branch lokal und remote löschen (git branch -D <source>; git push origin :<source>)
        Target-Branch pushen (git push)
        Bereinigung im Entwicklerteam anstoßen (git branch -d <source>; git fetch -p)

    Squash

        Source- und Target-Branch pullen ( git checkout <source>; git pull; git checkout <target>; git pull)
        Auf den Source-Branch wechseln (git checkout <source>)
        Rebase mit dem Target-Branch (git rebase <target>)
        Kompilieren (mvn clean install)
        Auf den Target-Branch wechseln (git checkout <target>)
        Source-Branch mit Squash mergen (git merge --squash <source>)
        Kommentare anpassen und Commiten (git commit)
        Nur bei Feature -> Master: Lokalen Source-Branch umbenennen und pushen (git branch -m <source> squashed/../..; git push origin --set-upstream squashed/../..)
        Remote Feature-Branch löschen (git push origin :<source>)
        Target-Branch pushen (git push)
        Bereinigung im Entwicklerteam anstoßen (git branch -d <source>; git fetch -p)

Beim Merge Squash Feature -> Master ist folgende Kommentarstruktur einzuhalten:

Req/Defxzy Titel

Beschreibung

<Squashed-Branch>
Commit-Hashes aller Commits auf dem Branch

Beispiel

Req115/116 Neuer Import-Prozess und GUI Aenderungen

Req115 1. Version der neuen Positionsverarbeitung
Req116 GUI-Aenderungen umgesetzt

squashed/2015.1/req115
commit c0bc7c13051968609f5097b94eae32f83daf2a55
commit 66462dedbe9e5cc1ed6b152275a8655150a1193d
commit 9e3f19553c77fbcacfb5f14c282e5684191bffd8
commit 508e2b4d6524df40cdf8f13230ccde399d100a68

Die Entscheidung, welche der beiden Vorgehensweisen Verwendung findet, ist mit Bedacht zu treffen. Und ist abhängig von der Anzahl, Sinnhaftigkeit und der Kommentarqualität der betroffenen Commits.

Wird nach Merge eines feature- oder refactoring-Branches noch eine Softwareänderung fuer dieses feature notwendig, darf der gemergte Branch nicht wieder verwendet werden. Entweder sind die notwendigen Änderungen per Defect zu tracken und auf einem defect-Branch zu commit oder es wird ein neuer Branch nach der folgenden Nomenklatur erstellt: [feature oder refactoring]/[Name des Releases]/[req oder def][Nr des Req oder Defects].[Laufende Nummer]. Beispiel: refactoring/2015.3/req359.1


VORSICHT!!! So KÖNNEN ALLE LOKALEN BRANCHES GELÖSCHT WERDEN git branch | grep -v "master" | xargs git branch -D
Releasepflege

Der bereits in den Swiss-Releases 2015.1 und 2015.2 angewendete Releasepflegeprozess wird nur in einigen Details angepasst, aber im Groben unverändert belassen.
Erstellung von Sub-Branches für Release-Fixes (4)

Wenn, nachdem die Hauptreleasebranches für ein Release gezogen wurden, es noch einmal zur Fixnotwendigkeit auf diesen Branches kommt, soll für jeden zu fixenden Defect (theoretisch auch Requirement) ein Sub-Branch erstellt werden. Folgendes Namensschema soll hier angewendet werden: release/[Name des parent Releasebranches]/[req oder def][Nr des Req oder Defects]

Die eigentliche Arbeit erfolgt wie im normalen Entwicklungsprozess ausschließlich auf Entwickler-Branches.

Beim Merge dieses Subbranches in den Parentreleasebranch sollen die unter 3 beschriebenen Vorgehen Anwendung finden.
Rückmerge von Fixes in die Parentbranches (5)

Für diesen Rückmerge wird kein Skript zur Verfügung gestellt, sondern nur die Verwendung des folgenden Befehls vorgeschrieben: git merge --no-commit --no-ff

Die Verwendung eines Aliases ist hier ratsam.

Auf Releasebranches darf kein rebase vor dem Merge erfolgen!
Cherry-Pick (6)

Ein Cherry-Pick darf nur in Ausnahmefällen durchgeführt werden. Und zwar nur dann, wenn eine Änderung auf 2 parallel gepflegten Releasebranches commited werden soll. In keinem anderen Szenario darf ein cherry-pick angewendet werden. 
