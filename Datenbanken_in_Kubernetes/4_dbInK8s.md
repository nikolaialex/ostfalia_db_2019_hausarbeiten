# Run Databases in Kubernetes

Erst mit der Einführung von Docker hat sich die Containerisierung durchgesetzt. Durch Docker als größtenteils verwendeten Standard von Containertechnologien ist es nämlich leicht Container zu erstellen, auszuliefern und in geringer Anzahl zu betreiben. [1] Aber gerade durch die Vozüge von Docker hat sich die Anzahl von produktiven Applikationen auf Dockerbasis schnell erhöht und ein manueller Betrieb ist nicht mehr effizient. Aus diesem Grund wurde der Einsatz von Orchestrator vorrangetrieben, der automatisiert eine Vielzahl von Container in unterschiedlichen Umgebungen organisiern, starten, stoppen und über Netzwerke hinweg zusammen arbeiten lassen kann. [1] Kubernetes ist so ein Orchestrator und kann Container unterschiedlichester Art betreiben.

Grundsätzlich eignet sich das Dateisystem eines Containers nicht für die dauerhafte Speicherung von Daten, Container sind i.A. zustandslos und werden im RAM der ausführenden Maschine ausgeführt. Eine erstellte Datei in dem Container ist also flüchtig und geht beim Stoppen des Containers verloren.
Besonders beim Einsatz einers Orchestartors wie Kubernetes, der beliebig Container z.B. je nach Auslastung der ausführenden Maschinen verschieben, starten und stoppen kann oder beim Ausfall einer ausführenden Maschine sogar muss, muss die Persistenz beim Betreiben von Datenbanken in Container besonders berücksichtigt werden.
Man spricht bei Betrieb von Datenbanken in Containern aufgrund ihrer Natur Daten persistent ablegen zu müssen von _Stateful Applications_, _Stateless Applications_ sind der weitaus einfachere Fall wie z.B. schon eine fertig als HTML-Dokument gebündelte Webseite, die über einen NGINX-Container ausgeliefert wird. Alles relevante in dem NGINX-Container ist statisch und ändert sich nicht. Es werden keine neuen wichtigen Dokumente in dem Container erstellt, die persistiert werden müssen. Sollen aber in einem Container zur Laufzeit erstellte Daten persistiert werden, so müssen diese auf externe Speicherorte außerhalb des Containers abgelegt werden. [2]

In einer gängigen Cloud-Umgebung wie z.B. Google Cloud oder Amazon Web Services hat man für die Persistierung von Daten i.d.R. zwei Möglichkeiten:

1. HDD/SSD der ausführende Maschine
2. Cloud-Speicher

## Deployments

```
Welche Arten gibt es Container insb. Datenbanken-Container mittels Kubernetes zu betreiben. Wie sieht sowas aus, z.B. anhand Definitions-Skripte? Welche Vor- und Nachteile ergeben sich daraus.
```

Via Deployments können Applikationen gestartet werden. Spezifizieren kann man die Art der Applikation, also das Docker Image und deren Anzahl per Replica Set. Das ist ein Controller, der stetig dafür sorgt, dass immer die spezifizierte Anzahl an Pods mit den Applikationen läuft.

> You describe a desired state in a Deployment, and the Deployment Controller
> changes the actual state to the desired state at a controlled rate. [4]

Deployments werden in Skripten spezifiziert, ein Beispiel dafür ist.

```yml
apiVersion: apps/v1

# Art der Ressource
kind: Deployment
metadata:
  # Name der Ressource
  name: nginx
spec:
  # Anzahl an Pods
  replicas: 3
  template:
    # Spezifikationen der Pods
    spec:
      # Definitionen der Container pro Pod
      containers:
        # Name des 1. Containers
        - name: nginx
          # Docker-Image des 1. Containers
          image: nginx:1.7.9
          # Offene Ports des 1. Containers
          ports:
            - containerPort: 80
```

## Stateful Sets

Seit der Version 1.9 von Kubernetes sind die StatefulSets in einer stabilen Form verfügbar. Sie ermöglichen in einem Kubernetes-Cluster die Bereitstellung von stateful-Anwendungen, welche die Nutzung von Datenbanken ermöglichen. Im Gegensatz zu Webanwendungen besitzen Transaktionsdatenbanken und deren Daten verschiedene Zustände. Diese Zustände müssen bei der Verwendung in Kubernetes bei allen Aktionen im Cluster berücksichtigt werden [3]. Als Cluster bezeichnet man alle Dienste, Container und Server.

## Vergleich

---

[<< Kubernetes](3_k8s.md) | [Inhaltsverzeichnis](inhaltsverzeichnis.md) | [Fazit >>](5_fazit.md)

---

## Literaturverzeichnis

| #   | Literatur                                                                                                                                                                                                                                                |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [1] | **Thomas Fricke (16.01.2018)**: _Kubernetes: Architektur und Einsatz – Eine Einführung mit Beispielen_, https://www.informatik-aktuell.de/entwicklung/methoden/kubernetes-architektur-und-einsatz-einfuehrung-mit-beispielen.html, aufgerufen 02.12.2019 |
| [2] | _Nichtflüchtige Speicher mit WordPress und MySQL verwenden_, https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk?hl=de, aufgerufen 01.11.2019                                                                                      |
| [3] | **Nicolas Bonorquez**: _Yes, You Can: Deploying Databases on Kubernetes with StatefulSets_, https://sweetcode.io/deploying-databases-kubernetes-statefulsets/, aufgerufen 28.11.2019                                                                     |
| [4] |                                                                                                                                                                                                                                                          |
