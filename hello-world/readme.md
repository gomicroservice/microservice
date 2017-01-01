# Hello World - Microservice

## Funktion

Dieser Service liefert beim Aufruf auf Contextroot "/" ein JSON-Objekt zurück 
mit einer ID, einem String "Hello World!" und eine Liste der Primzahlen unter 
1.000 zurück. Der Service ist damit zur Laufzeit abhängig vom Service 
[primes](../primes). Wenn der Service nicht verfügbar ist, dann wird eine 
Fehlermeldung geliefert.

## Implementierung

Der Aufruf des Primes-Services erfolgt mittels JSON. Das Objekt zum 
deserialisieren ist in diesem Projekt implementiert, weil keine binäre 
Abhängigkeit zum Projekt primes erwünscht ist.

## Test

Dieser Service kann einfach durch Aufruf der URL `http://<host>:<port>/`
aufgerufen werden oder aus einer Bash mit `curl <host>:<port>`. In Fehlerfall 
kann der HTTP-Header mit `curl -i <host>:<port>` ausgegeben werden.

Der Wert "World" ist der Default-Value für den Parameter "name". Der Parameter
kann auch einfach als HTTP-Parameter gesetzt werden.

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
