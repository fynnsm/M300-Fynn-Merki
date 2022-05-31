# Docker

hub.docker.com

Hier sind alle Images hinterlegt, ein Image ist grob gesagt eine Applikation, welche in einem Container laufen gelassen werden kann.
Auf der Webseite kann man unter Explore alle diese Images ansehen, Verified Image heisst wenn es vom hersteller selber, oder von Docker überprüft wurde, kurz gesagt, diese Images sind sicher.
Wenn man sein Image gefunden hat kann man denn Befehl sehen um es zu pullen. Ein Image pullen, heist eigentlich einfach es herunterzuladen un lokal zu haben. Dieser Schritt ist notwendig um es später laufen und verändern zu können.


Mit:
```docker pull ubuntu/mysql```

Laden wir also jetzt unser Image herunter, dieser Schritt ist eigentlich nicht nötig, da später ein Image welches lokal noch nicht existiert, aber ausgeführt werden sollte automatisch heruntergeladen wird.
Hinter dem Namen des Images kann man mit einem ":" getrennt noch die version des images angeben, falls man nichts angiebt wird die neuste Version automatisch heruntergeladen.

Nach dem Download kann mit dem Befehl:
docker images
herausgefunden werden, welche Images lokal schon vorhanden sind und die version ist auch sichtbar.

Anschliessend wird mit:
```docker run -d --name mysql-container -e TZ=UTC -p 30306:3306 -e MYSQL_ROOT_PASSWORD=My:S3cr3t/ ubuntu/mysql:8.0-22.04_beta```

Das Image in einem Container gestartet. Falls das Image hier noch nict heruntergeladen ist wird es jetzt automatisch gepullt.
Der Run Befehl besteht aus folgenden Teilen:
- ```docker run``` um zu sagen was man überhaupt will container stoppen oder starten
- ```-d``` detach um wieder zurück auf die Commandozeile zu kommen
- ```-- name``` um den Namen des Containers anzugeben
- ```-e TZ=``` um die Timezone des Servers zu setzten 
- ```-e MYSQL_ROOT_PASSWORD=``` um das Root Passwort des Sql Servers zu definieren
- ```-p``` um das Port forwarding einzurichten also anfragen welche der Host auf "30306" bekommt werden intern auf den Port "3306" weitergeleitet, auf welchem der Docker Deamon wartet. wie bei einer VM
- und hinten noch das image mit ":" um die Version anzugeben

Mit:
```docker exec -it mysql-container bash```

kann man auf den Container selber verbinden, wie wenn man auf eine VM verbindet, wenn man connectet ist, kann man unter /var/lib/mysql seine Tables sehen

Um den Container zu stoppen, "herunterzufahren":
```docker stop fynnsql```
