# Docker

hub.docker.com

Hier sind alle Images hinterlegt, ein Image ist grob gesagt eine Applikation, welche in einem Container laufen gelassen werden kann.
Auf der Webseite kann man unter Explore alle diese Images ansehen, Verified Image heisst wenn es vom hersteller selber, oder von Docker überprüft wurde, kurz gesagt, diese Images sind sicher.
Wenn man sein Image gefunden hat kann man denn Befehl sehen um es zu pullen. Ein Image pullen, heist eigentlich einfach es herunterzuladen un lokal zu haben. Dieser Schritt ist notwendig um es später laufen und verändern zu können.

![docker image](images/docker.png)


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




## Docker Container mit externem Speichermedium
```docker volume```
zeigt alle Docker Volumen an 

```docker volume prune```
Löscht alle Volumen, welche nicht aktiv gebraucht werden

```docker volume create mydbstore```
erstellt ein neues Docker volumen

```docker volume ls```
zeigt alle Volumen an

```docker volume inspect mydbstore```
Um Das Volumen, welches wir erstellt haben zu überprüfen. Auf Mountpoint sieht man wo es auf dem Host system abgelegt ist.

```docker run -d --name mysql-container -v mydbstore:/var/lib/mysql -p 30306:3306 -e MYSQL_ROOT_PASSWORD=My:S3cr3t/ ubuntu/mysql:8.0-22.04_beta```
 Container wieder erstellen und mit ```-v mydbstore:/var/lib/mysql``` der Teil nach dem Doppelpunkt gibt an welcher ordner auf dem Container dieses Volumen sein soll.

## ubuntu image/build

Container erstellen:
```docker run -it --name <name> --hostname <name> ubuntu```

installations history:
```docker history <image>```

Sehen welche Container aktiv sind:
```docker ps ```


ind die den Container selber gehen:
```docker exec -it <name> bash```

container updaten 
```apt-get udpate```

zwei programme installiern:
```apt-get install -y cowsay fortune```

testen ob installiert ist:
```/usr/games/fortune | /usr/games/cowsay```

Ergebnis:
``` _________________________________________
/ The Least Perceptive Literary Critic    \
|                                         |
| The most important critic in our field  |
| of study is Lord Halifax. A most        |
| individual judge of poetry, he once     |
| invited Alexander Pope round to give a  |
| public reading of his latest poem.      |
|                                         |
| Pope, the leading poet of his day, was  |
| greatly surprised when Lord Halifax     |
| stopped him four or five times and      |
| said, "I beg your pardon, Mr. Pope, but |
| there is something in that passage that |
| does not quite please me."              |
|                                         |
| Pope was rendered speechless, as this   |
| fine critic suggested sizeable and      |
| unwise emendations to his latest        |
| masterpiece. "Be so good as to mark the |
| place and consider at your leisure. I'm |
| sure you can give it a better turn."    |
|                                         |
| After the reading, a good friend of     |
| Lord Halifax, a certain Dr. Garth, took |
| the stunned Pope to one side. "There is |
| no need to touch the lines," he said.   |
| "All you need do is leave them just as  |
| they are, call on Lord Halifax two or   |
| three months hence, thank him for his   |
| kind observation on those passages, and |
| then read them to him as altered. I     |
| have known him much longer than you     |
| have, and will be answerable for the    |
| event."                                 |
|                                         |
| Pope took his advice, called on Lord    |
| Halifax and read the poem exactly as it |
| was before. His unique critical         |
| faculties had lost none of their edge.  |
| "Ay", he commented, "now they are       |
| perfectly right. Nothing can be         |
| better."                                |
|                                         |
| -- Stephen Pile, "The Book of Heroic    |
\ Failures"                               /
 -----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

wieder aus dem Contianer gehen: 
```exit```

Docker image erstellen vom Container, ersteer Name nme des Containers, zweiter Name de neuen images:
```docker commit <name> img/<name>```

Images anshenen:
```docker images```


Ergebnis:
```
REPOSITORY                                                       TAG              IMAGE ID       CREATED          SIZE
img/unbuntuimg                                                   latest           b6f6a9920269   56 seconds ago   162MB
```

nun kann das Image wieder gestartet werden:
```docker run -it --name <name> --hostname <name> img/<name> bash```

(mit bash ist man direkt in der Bash)


## Docker Network

wir starten wieder unseren sql container:<br>
```docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=secret -p 3036:3036 mysql```

wir sehen uns an welche netzwerke schon vorhanden sind:<br>
```docker network ls```

Ergebnis:
```
NETWORK ID     NAME      DRIVER    SCOPE
dae535ca81f8   bridge    bridge    local
ee5ea6d1a081   host      host      local
550d4d071b12   none      null      local
```

bridge ansehen, z.B. Netzwork oder Gateway:<br>

```docker network inspect bridge```

neues Netzwerk erstellen:<br>

```docker network create mynetwork```

jetzt wieder die Netzwerke auflisten:<br>
```docker network ls```

Ergebnis:
```
NETWORK ID     NAME        DRIVER    SCOPE
dae535ca81f8   bridge      bridge    local
ee5ea6d1a081   host        host      local
ad0d2e747c1b   mynetwork   bridge    local
550d4d071b12   none        null      local
```

ansehen:
```Docker inspect mynetwork```

```

[
    {
        "Name": "mynetwork",
        "Id": "ad0d2e747c1b9ccdde94fb61d32c24231f8cf89340632c3b6ddd987df564e266",
        "Created": "2022-06-14T06:33:06.664955538Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

Alle nicht gebrauchten docNetzwerke löschen:
```docker network prune```


## Docker image mit Docker hub

Als erstes einen Docker HUB acc erstellen auf: hub.docker.com

Mit folgenden Befehlen dad Gitlab von Herr Calisto lokal kopieren 

```$ mkdir TEMP_Docker ``` Unterverzeichnis "TEMP_Docker" erstellen <br>
```$ cd TEMP_Docker```   Ins Unterverzeichnis "TEMP_Docker" wechseln <br>
```$ git clone https://gitlab.com/ser-cal/Container-CAL-webapp_v1.git ```  Repo klonen<br>
```$ cd Container-CAL-webapp-v1/ ``` ins Repo-Unterverzeichnis hüpfen<br>
```$ cd APP  ``` Ins Unte

Sobald man am richtigen ort ist kann man das File ```views/home.pug``` anpassen, wichtig hinter ein H1 schreiben.

nach dem ein Image erstellen, name durch namen ersetzten.<br>
 ```$ docker image build -t <name>/webapp_one:1.0 .```

 jetzt sich mit seinem dockker Account einlogen:<br>

 ```$ docker login --username=<name>```

 Danach das image auf docker hub pushen:<br>

 ```$ docker image push <name>do/webapp_one:1.0```

 Jetzt nur noch die den Container starten: <br>
``` docker container run -d --name cal-web -p 8080:8080 marcellocalisto/webapp_one:1.0 ```