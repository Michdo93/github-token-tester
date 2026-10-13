# github-token-tester
Einfaches Python-Skript, mit dem man überprüfen kann, ob sein GitHub-Token noch funktioniert und aktuell ist.

Wer kennt es nicht? Man verlegt seinen Token, man hat mehrere notiert oder man findet in alten Anwendungen oder Skripten ein Token. Bevor man neu generiert und möglicherweise dann irgendwelche Skripte und Systeme unbrauchbar macht, weil man vergisst, bei allen auf das neue Token umzustellen, kann man lieber testen, ob ein Token noch funktioniert. Da manche Anwendungen jedoch komplexer sind oder mehrere Fehler werfen, kann man sich aber häufig genug nicht sicher sein, ob ein falscher Token die Ursache ist. Mit diesem einfachen Code kann man überprüfen, ob dieser Token grundsätzlich noch funktioniert.

## Code

Hier der Code:

```
import urllib.request
import json

# Trage hier einen deiner Tokens ein
TOKEN = "DEIN_TOKEN_HIER_EINTRAGEN"

url = "https://api.github.com/user"
headers = {
    "Authorization": f"Bearer {TOKEN}",
    "Accept": "application/vnd.github+json",
    "User-Agent": "Python-Token-Tester"
}

req = urllib.request.Request(url, headers=headers)

try:
    with urllib.request.urlopen(req) as response:
        data = json.loads(response.read().decode())
        print("✅ Token ist GÜLTIG!")
        print(f"Eingeloggt als GitHub-User: {data.get('login')} (Name: {data.get('name')})")
except urllib.error.HTTPError as e:
    print(f"❌ Token ist UNGÜLTIG oder abgelaufen (Fehlercode: {e.code})")
    print(e.read().decode())
except Exception as e:
    print(f"❌ Ein Fehler ist aufgetreten: {e}")
```

Entsprechend sollte man bei der Konstante `TOKEN` seinen zu testenden `Fine-grained Token (github_pat_XXX)` oder seinen zu testenden `Classic Token (ghp_YYY)` eintragen und das Programm ausführen.
