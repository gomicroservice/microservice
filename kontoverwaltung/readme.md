# Kontoverwaltung - Microservice

## Funktion

Dieser Service erzeugt Events und publiziert sie in eine Messagequeue. 

### Events
Welche Events?
Einzelnes Event oder viele?

Dieser Microservice bietet folgende Funktionalität an:
* UI zu Anlegen, Ändern und Löschen von Konten per Event (asynchron).
* Zufallsgesteuerter Generator von Umsätzen je aktives Konto.

Zur Vereinfachung hat ein Konto nur einen Inhaber, der per Namen identifiziert 
wird und _keine_ Kundennummer hat, weil derzeit kein System zur Verwaltung von 
Kunden vorliegt.



## Implementierung

### Konfiguration

RabbitMQ starten

`sudo rabbitmq-server start|stop|staus`

oder

`sudo rabbitmqctl start|stop|status`

## Test

Dieser Service kann einfach durch Aufruf der URL `http://<host>:<port>/`
aufgerufen werden oder aus einer Bash mit `curl <host>:<port>`. In Fehlerfall 
kann der HTTP-Header mit `curl -i <host>:<port>` ausgegeben werden.

````
curl localhost:9090/?name=Jürgen | json_pp
````

Liefert ein JSON-Objekt der Art:

````
{
  "id": 1,
  "name": "Jürgen",
  "primes": [2, 3, 5, ... 997]
}
````

## Actuator

Spring Boot liefert sog. Actuators mit. Wir aktivieren die Actuators, weil sie
von die Netflix-Services für die Überwachung benötigt werden. Der für uns 
wichtigste Actuator ist der Health-Endpoint. Er ist unter `/health` erreichbar.

````
curl localhost:9090/health | json_pp
````

liefert etwas in der Art

````
{
    "status": "UP",
    "diskSpace": {
        "status": "UP",
        "total": 511397851136,
        "free": 200806346752,
        "threshold": 10485760
    }
}
````

## Profile

Das Projekt verfügt über zwei Profile:

* local
* docker

### local

In diesem Profil laufen alle Services auf localhost und die Ports werden so
umgebogen, dass sich die Services nicht gegenseitig behindern. Die
Konfiguration erfolgt in application-local.properties

Um dieses Profil zu starten muss _kein_ Parameter gesetzt / übergeben werden.

### docker

In diesem Profil laufen alle Service in einem eigenen Docker-Container. Die
Services haben also eine eigene IP-Adresse und können alle auf dem jeweils
gleichen Port laufen, weil sie ja isoliert voneinander sind.

Um dieses Profil zu starten muss das Profil beim Start gesetzt werden. Dazu
stehen zwei Varianten zur Verfügung:

````
java -jar hello-world-0.0.1-SNAPSHOT.jar --spring.profiles.active=docker
````

oder

````
java -jar -Dspring.profiles.active=docker hello-world-0.0.1-SNAPSHOT.jar
````


# Todo
* Ab 10_Docker_Images_skaliert funktioniert der Absprung aus Eureka nicht mehr.
  Eureka lenkt auf die IP-Adresse:Serverport um. Leider ist der Port nicht in
  Docker freigegeben. Wünschenswert wäre, dass der Zugriff via Zuul
  funktioniert.
