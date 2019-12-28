# How to Run DB in Kubernetes

Erst mit der Einführung von Docker hat sich die Containerisierung durchgesetzt. Mit zunehmender Anzahl von Containern wurde eine Orchestrierung notwendig. Orchestrierung bedeutet, dass Container organisiert, gestartet und gestoppt werden müssen. Für diese Aufgabe wurde Kubernetes erstellt. Die Orchestrierung von Datenbanken ist technisch möglich aber auch für jede Datenbank individuell. Die Funktionsfähigkeit der eingesetzten Datenbanken muss für jeden einzelnen Hersteller sorgfältig getestet werden [9].

Grundsätzlich eignet sich das Dateisystem eines Containers nicht für die dauerhafte Speicherung von Daten. Die durch Kubernetes ausgeführten Container können gelöscht, entfernt oder neu erstellt werden. Der Cluster-Manager kann diese Aktionen jederzeit auslösen. Dies kann aufgrund von Knotenausfällen oder nicht mehr benötigten Ressourcen erfolgen. In einem solchen Fall gehen alle im Container gespeicherten Daten verloren. Sollen die Daten erhalten bleiben müssen diese auf persistenten Festplatten außerhalb der Container gespeichert werden [6].

Seit der Version 1.9 von Kubernetes sind die StatefulSets in einer stabilen Form verfügbar. Sie ermöglichen in einem Kubernetes-Cluster die Bereitstellung von stateful-Anwendungen, welche die Nutzung von Datenbanken ermöglichen. Im Gegensatz zu Webanwendungen besitzen Transaktionsdatenbanken und deren Daten verschiedene Zustände. Diese Zustände müssen bei der Verwendung in Kubernetes bei allen Aktionen im Cluster berücksichtigt werden [8]. Als Cluster bezeichnet man alle Dienste, Container und Server [2].

## Deployments

```
Welche Arten gibt es Container insb. Datenbanken-Container mittels Kubernetes zu betreiben. Wie sieht sowas aus, z.B. anhand Definitions-Skripte? Welche Vor- und Nachteile ergeben sich daraus.
```

Per Deployments können Applikationen gestartet werden. Spezifizieren kann man die Art der Applikation, also das Docker Image und deren Anzahl per Replica Set. Das ist ein Controller, der stetig dafür sorgt, dass immer die spezifizierte Anzahl an Pods mit den Applikationen läuft.

> You describe a desired state in a Deployment, and the Deployment Controller
> changes the actual state to the desired state at a controlled rate.

Deployments werden in Skripten spezifiziert, ein Beispiel dafür ist.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.7.9
          ports:
            - containerPort: 80
```

## Stateful Sets

## Vergleich
