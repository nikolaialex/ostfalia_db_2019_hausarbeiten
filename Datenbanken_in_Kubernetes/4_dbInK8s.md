# Run Databases in Kubernetes

Erst mit der Einführung von Docker hat sich die Containerisierung durchgesetzt. Durch Docker als größtenteils verwendeten Standard von Containertechnologien ist es nämlich leicht Container zu erstellen, auszuliefern und in geringer Anzahl zu betreiben. [1] Aber gerade durch die Vozüge von Docker hat sich die Anzahl von produktiven Applikationen auf Dockerbasis schnell erhöht und ein manueller Betrieb ist nicht mehr effizient. Aus diesem Grund wurde der Einsatz von Orchestrator vorrangetrieben, der automatisiert eine Vielzahl von Container in unterschiedlichen Umgebungen organisiern, starten, stoppen und über Netzwerke hinweg zusammen arbeiten lassen kann. [1] Kubernetes ist so ein Orchestrator und kann Container unterschiedlichester Art betreiben.

Grundsätzlich eignet sich das Dateisystem eines Containers nicht für die dauerhafte Speicherung von Daten, Container sind i.A. zustandslos und werden im RAM der ausführenden Maschine ausgeführt. Eine erstellte Datei in dem Container ist also flüchtig und geht beim Stoppen des Containers verloren.

Besonders beim Einsatz einers Orchestartors wie Kubernetes, der beliebig Container z.B. je nach Auslastung der ausführenden Maschinen verschieben, starten und stoppen kann oder sogar beim Ausfall einer ausführenden Maschine muss, Berücksichtigung der Persistenz beim Betreiben von Datenbanken in Container ist also besonders wichtig.

## Stateful und Stateless Applications

Man spricht bei Betrieb von Datenbanken in Containern aufgrund ihrer Natur Daten persistent ablegen zu müssen von _Stateful Applications_, _Stateless Applications_ als Gegenstück ist die weitaus einfache Art von Applikation zu händeln. Eine _Stateless Application_ ist z.B. eine NGINX-Server, der eine schon fertig gebündelte Webseite bei Aufruf über seine URL im Browser ausliefern soll. Alles relevante in dem NGINX-Container ist statisch und ändert sich nicht. Es werden keine neuen wichtigen Dokumente in dem Container erstellt, die persistiert werden müssen. Ein [Dockerfile](https://github.com/TinkeringAround/nginx-docker-k8s/blob/master/Dockerfile) für so ein Image ist entsprechend einfach und benötigt keine Anbindung zu externen Volumes oder Verzeichnisquellen für eine Persistierung.

```Dockerfile
# Small NGINX Base Image
FROM nginx:alpine

# Deliver always index.html Page on Incoming Request on Default Port 80
COPY /src/index.html /usr/share/nginx/html/index.html
```

Sollen aber in einem Container zur Laufzeit erstellte Daten persistiert werden, so müssen diese auf externe Speicherorte außerhalb des Containers abgelegt werden. [2]

In einer gängigen Cloud-Umgebung wie z.B. Google Cloud oder Amazon Web Services hat man für die Persistierung von Daten i.d.R. zwei Möglichkeiten:

1. HDD/SSD der ausführende Maschine
2. Cloud-Speicher

## Storage und Persistenz

Wie im Abschnitt [Docker](2_docker.md) beschrieben kann ein Container mit dem Dateiverzeichnis der darunterliegenden ausführenden Maschine verknüpft werden und eigentlich im RAM/im Container erstellte Dateien stattdessen in diesem Dateiverzeichnis der ausführenden Maschine in ihrer HDD/SSD ablegen. Diese Persistierung mag für den einfachen Betrieb von Containern in einer Virtuellen Maschine ausreichen, ist für den Einsatz im Rahmen von Kubernetes aber keine Option.

Kubernetes führt eine Lastverteilung der laufenden Applikationen auf den zur Verfügugen stehenden Virtuellen Maschinen als Host aus. Steigt die Last bei einer VM als Host an, so können Applikationen auf anderen umgezogen werden. Dafür startet Kubernetes zunächst neue Container und wartet bis diese stabil laufen. Anschließend stoppt er die Container auf den höher belasteten Host. Für den Entwickler und Betreiber von Applikationen soll es bei Verwendung von Kubernetes egal sein auf welchem Host die Anwendung ausgeführt wird. Wichtig ist nur, dass Anwendung auch so ausgelegt werden, dann sie möglichst einfach und schnell gestoppt und gestartet werden können und dann reibungslos weiter funktionieren. Das stellt neue Anforderungen an die Art der Entwicklung von Software.

Die zweite Variante ist die Verwendung von externen Cloud-Speicher, die wie eine externe Festplatte für verschiedene Anwendungen per _"Plug and Play"_ verwendbar gemacht werden kann. In Kubernetes sind diese Speicherorte unter dem Konzept _Volumes_ bekannt.

- Wie funktioniert wenn Container Volume braucht (Persistent volume claim)
- Was passiert mit dem Volume wenn Container stirbt?

```
Mit Kubernetes 1.3 wurde die automatische Speicherplatzzuordnung (Provisioning von Storage) für Container bereitgestellt. Der Storage wird als Persistent Volumes von Kubernetes bereitgestellt. Abhängig von der zugrunde liegenden Plattform, gibt es diese in verschieden Ausführungen. Läuft Kubernetes in der Google Cloud, bietet es die GCEPersistentDisk, bei Amazon den ElasticBlockStore und auf Azure das AzureFile oder die AzureDisk. Nachteilig dabei ist, dass sie die Abstraktion und Orchestrierung durch Kubernetes zunichtemachen und an den jeweiligen Cloudprovider binden. Es existieren Projekte wie bespielsweise “Rook"ein, die ebenfalls ein verteiltes Dateisystem betreiben, dies aber direkt auf dem Kubernetes-Cluster aufsetzen und somit weiteres Management überflüssig machen.

```

## TODO: Arten Container in einem Kubernetes-Umfeld zu starten

- Text ergänzen!

### Deployments

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

### Stateful Sets

Seit der Version 1.9 von Kubernetes sind die StatefulSets in einer stabilen Form verfügbar. Sie ermöglichen in einem Kubernetes-Cluster die Bereitstellung von stateful-Anwendungen, welche die Nutzung von Datenbanken ermöglichen. Im Gegensatz zu Webanwendungen besitzen Transaktionsdatenbanken und deren Daten verschiedene Zustände. Diese Zustände müssen bei der Verwendung in Kubernetes bei allen Aktionen im Cluster berücksichtigt werden [3]. Als Cluster bezeichnet man alle Dienste, Container und Server.

### Vergleich

| #   | Literatur                                                                                                                                                                                                                                                |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [1] | **Thomas Fricke (16.01.2018)**: _Kubernetes: Architektur und Einsatz – Eine Einführung mit Beispielen_, https://www.informatik-aktuell.de/entwicklung/methoden/kubernetes-architektur-und-einsatz-einfuehrung-mit-beispielen.html, aufgerufen 02.12.2019 |
| [2] | _Nichtflüchtige Speicher mit WordPress und MySQL verwenden_, https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk?hl=de, aufgerufen 01.11.2019                                                                                      |
| [3] | **Nicolas Bonorquez**: _Yes, You Can: Deploying Databases on Kubernetes with StatefulSets_, https://sweetcode.io/deploying-databases-kubernetes-statefulsets/, aufgerufen 28.11.2019                                                                     |
| [4] |                                                                                                                                                                                                                                                          |

---

[<< Kubernetes](3_k8s.md) | [Inhaltsverzeichnis](0_title.md) | [Fazit >>](5_fazit.md)

---
