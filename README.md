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
