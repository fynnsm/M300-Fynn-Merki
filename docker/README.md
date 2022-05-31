#Docker

hub.docker.com

Hier sind alle Images hinterlegt, ein Image ist grob gesagt eine Applikation, welche in einem Container laufen gelassen werden kann.
Auf der Webseite kann man unter Explore alle diese Images ansehen, Verified Image heisst wenn es vom hersteller selber, oder von Docker überprüft wurde, kurz gesagt, diese Images sind sicher.
Wenn man sein Image gefunden hat kann man denn Befehl sehen um es zu pullen. Ein Image pullen, heist eigentlich einfach es herunterzuladen un lokal zu haben. Dieser Schritt ist notwendig um es später laufen und verändern zu können.


Mit:
docker pull ubuntu/mysql
Laden wir also jetzt unser Image herunter, dieser Schritt ist eigentlich nicht nötig, da später ein Image welches lokal noch nicht existiert, aber ausgeführt werden sollte automatisch heruntergeladen wird.
Hinter dem Namen des Images kann man mit einem ":" getrennt noch die version des images angeben, falls man nichts angiebt wird die neuste Version automatisch heruntergeladen.

Nach dem Download kann mit dem Befehl:
docker images
herausgefunden werden, welche Images lokal schon vorhanden sind und die version ist auch sichtbar


docker run -d --name mysql-container -e TZ=UTC -p 30306:3306 -e MYSQL_ROOT_PASSWORD=My:S3cr3t/ ubuntu/mysql:8.0-22.04_beta



docker stop fynnsql

docker exec -it mysql-container bash