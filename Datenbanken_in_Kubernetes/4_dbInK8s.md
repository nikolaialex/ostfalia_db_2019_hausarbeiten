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

1. HDD/SSD der ausführende Maschine (Node)
2. Cloud-Speicher als Volume-Ressource in Kubernetes

## Storage und Persistenz

Wie im Abschnitt [Docker](2_docker.md) beschrieben kann ein Container mit dem Dateiverzeichnis der darunterliegenden ausführenden Maschine verknüpft werden und eigentlich im RAM/im Container erstellte Dateien stattdessen in diesem Dateiverzeichnis auf der HDD/SSD der ausführenden Maschine ablegen. Diese Persistierung mag für den einfachen Betrieb von Containern in einer Virtuellen Maschine ausreichen, ist für den Einsatz im Rahmen von Kubernetes aber keine Option.

Kubernetes führt eine Lastverteilung der laufenden Applikationen auf den zur Verfügugen stehenden Virtuellen Maschinen als Host, in Kubernetes Node genannt, aus. Steigt die Last auf einem Node an, so können Applikationen auf andere im Cluster zur Verfügung stehende Nodes umgezogen werden. Dafür startet Kubernetes zunächst einen neuen Container auf einem weniger belasteten Node und wartet bis diese stabil laufen. Anschließend stoppt er die Container auf den höher belasteten Node. Nodeabhängige Datenbestände von Container werden nicht mit umgezogen, da i.A. Kubernetes nicht weiß welche Applikationen welche Daten ablegen und wie wichtig diese sind. Für den Entwickler und Betreiber von Applikationen soll es bei Verwendung von Kubernetes außerdem auch unrelevant sein auf welchem Node die Anwendung ausgeführt wird. Wichtig ist nur, dass Anwendung vom Entwickler auch so ausgelegt werden, dann sie möglichst einfach und schnell gestoppt und gestartet werden können und dann reibungslos weiter funktionieren. Das stellt neue Anforderungen an die Art der Entwicklung von Software.

Die zweite Variante ist die Verwendung von externen Cloud-Speicher, die wie eine externe Festplatte für verschiedene Anwendungen per _"Plug and Play"_ verwendbar gemacht werden kann. Mit Kubernetes 1.3 wurde die automatische Speicherplatzzuordnung (Provisioning von Storage) für Container bereitgestellt. Der Storage wird als _Persistent Volumes_ von Kubernetes bereitgestellt und kann mit Pods bzw. darin laufende Container verknüpft werden. Cloud-Speicher sind wie Cloud-Infrastruktur selbst betreiberabhängig und richtet sich nach deren Angebot. So variieren nicht nur verschiedene Arten von einsetzbaren VMs für das Kuberentes Cluster mit unterschiedlichen Specs, sondern auch die Cloud-Speicher unterscheiden sich nach Art HDD/SSD und provisionierbarer Größe. Beim Starten von Applicationen und verknüpfen mit Volumes kann die Art des Volumes mit der betreiberspezifischen _Storage Class_ angegeben werden. Läuft Kubernetes z.B. in der Google Cloud, bietet es die GCEPersistentDisk, bei Amazon den ElasticBlockStore und auf Azure das AzureFile oder die AzureDisk. Bestimmte Anbieter bietet Storage Classes an, die sogar direkt eine Replikation von Daten ermöglichen und damit höhere Anforderungen an die Ausfallsicherheit gewährleisten. Es existieren Projekte wie bespielsweise “Rook", die ein verteiltes Dateisystem betreiben, dies aber direkt auf dem Kubernetes-Cluster aufsetzen und somit weiteres Management überflüssig machen.

Volumes sind externe Cloud-Speicherressourcen und sind damit im Gegensatz zu der ersten Variante unabhängig von jeglicher Infrastruktur des Kubernetes Clusters. Stürzt z.B. eine laufende Mongo Datenbank ab, so sind bei richtiger Verknüpfung von Container und Volume, die Daten auf dem Volume sicher. Der Container wird einfach neu gestartet, und zwar auch auf einem anderen Node als zuvor, und wieder mit dem Volume gelinkt. Die Datenbank kann dann direkt weiterarbeiten. Aus diesem Grund ist der generelle Ansatz von Volumes im Kontext von Kubernetes für die Persistierung von Daten mit Abstand die bessere Variante. Einige entscheidene Vorteile und Möglichkeiten von Volumes zusammengefasst:

- Komplexe verteilte Datensysteme
- Replikationsmechanismen
- Gezielte Backup-Routinen
- Automatische Speichervergrößerung
- Dynamische Provisionierung on Demand
- Betreiber von Cloud-Speicher ist verantwortlich für Betrieb (keine Administration)

Die Ablage von Daten auf den Nodes selber ist meistens sehr aufwendig, da meist werden Kubernetes Cluster nicht selbst auf eigenst partitionierten VM oder Bare Metal Maschinen eingerichtet, sondern die Hosted Kubernetes Services von den oben genannten Cloud-Plattformen verwendet. Dadurch hat man meist nur einen eingeschränkten Zugriff zu den Nodes selbst und es ist schwer die oben genannten verfügbaren Vorteile von Volumes umzusetzen.

### Persistent Volume Claim

Container bzw. Anwednungen können in Kubernetes entweder per CLI (Kommandozeilen-Interface), im Kubernetes Cluster deploytes Dashboard oder per Skripte gestartet werden. Die letzte Variante ist die bevorzugte, da Skripte (YML- oder JSON-Datei) versioniert und für die Automatisierung leicht integrierbar sind.

Wenn man eine Datenbank als Container in einem Pod starten möchte ist der reproduzierbarste Weg die Erstellung eines entsprechenden Skriptes, wo der Pod samt auszuführenden Container spezifiziert wird. Ein minimales Skript für das Ausführen einer MongoDB ist nachfolgend dargestellt.

```yml
apiVersion: v1
kind: Pod
metadata:
  # Pod Name
  name: mongodb
spec:
  # 1. Container in Pod
  containers:
    - name: mongodb
      # Docker Image with Version-Tag
      image: mongo:4
      # Opened and named Container Ports
      ports:
        - containerPort: 27017
          name: http
      # Volume Mapping from Container to Kubernetes Volume via PVC
      volumes:
        - name: mongo-persistent-volume
          persistentVolumeClaim:
            claimName: mongo-persistent-volume-claim
```

Neben dem Namen und den geöffneten Container-Pors wird analog wie zu Docker das interne Dateiverzeichnis der Mongo Datenbank mit einem Kubernetes Volume gemappt/verknüpft. Dieses Volume `mongo-persistent-volume` ist wiederum über ein _Persistent Volume Claim_ (PVC) mit einem tatsächlich vom Cloud-Betreiber provisionierte Speicher verknüpft (link).

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  # PVC Name
  name: mongo-persistent-volume-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      # Requested Storage Size = 10GB
      storage: 10Gi
      # Selected Storage Class from Provider Ionos
  storageClassName: ionos-enterprise-hdd
```

Wie bereits beschrieben sind provisionierte Volume einer Storage Class unabhängig von jeglichen Workload im Kubernetes Cluster. Ein Container kann also kein Volume direkt _besitzen_, sondern leglich ein Volume anfordern (claim). Kubernetes sorgt dann dafür, dass der Container beim Schreiben auf dieses externe Verzeichnis zugreifen kann und stellt dafür die notwendige Verbindung her. Wenn ein PVC erstellt wird und kein zugehöriges Volume existiert, wird dieses dynamisch provisioniert.

Zu einem partitioniertem Volume kann auch nur ein PVC existieren, beliebig viele Container können allerdings ein PVC verwenden, um Zugriff zu dem verknüpften Volume zu erhalten und Daten zu lesen und zu schreiben.

## Running Workload in Kubernetes

In diesem Abschnitt werden nun drei Arten/Typen (kind) beschrieben, wie eine Anwendung (Workload), hier mit Fokus auf Datenbanken, in einem Kubernetes Cluster gestartet und betrieben werden kann.

- Pod
- Deployment
- Stateful Set

Dabei wird lediglich nur der bevorzugte Fall, die Verwendung von Cloud-Speichern/Volumes, berücksichtigt. Kubernetes bietet noch weitere Variante wie Anwendungen betrieben werden können, diese sollen hier allerdings nicht weiter behandelt werden.

### Pods

`pod` ist die einfachste Art ein oder mehrere Container in einem Kubernetes Cluster zu starten. Ein Beispiel für das Deployment von einem Pod ist im Abschnitt PVC dargestellt. Ein Pod hat keinen Controller, der dafür sorgt, dass der Pod bei Absturz der Anwednung auch wieder neu gestartet wird. Eine Datenbank, die also auf diese Weise gestartet und via PVC mit einem Volume verknüpft wird, verliert beim Absturz aufgrund des Volumes zwar keine Daten, wird aber auch nicht neu gestartet. Auch kann über diese Art auch immer nur ein Pod gestartet werden, werden mehrere Replicas des geleichen Types benötigt, so ist auch hier z.B. ein Replication Controller und damit ein `deployment` notwendig.

### Deployments

Der Typ `deployment` ist generell für komplexere Stateless oder einfache Stateful Applications gedacht. Ein Replication Controller sorgt dafür, dass eine bestimmte Anzahl von identischen Pods laufen. Die Pods werden analog zu dem `pod` Deployment spezifiziert.

> _"You describe a desired state in a Deployment, and the Deployment Controller
> changes the actual state to the desired state at a controlled rate."_ [4]

Für kleine Anwendungen, z.B. eine Website (klassischer WordPress-Stack) bestehenden aus PHP-Server in einem Pod und einer Datenbank in einem anderen Pod kann dieser Typ `deployment` sehr gut verwendet werden. Es werden keine sehr hohen Ansprüche an eine 100%-Verfügbarkeit noch an die Partitionstoleranz gemäß CAP-Theorem gestellt, da das Cluster an sich meist sowieso nicht hochverfübar gestaltet ist und nur aus wenigen Nodes besteht. Die Konsistenz ist allerdings gewährleistet, es gibt nur eine schreibende bzw. lesende Instanz, ein Pod mit dem PHP-Server. Nachfolgend ist ein Beispiel für ein MySQL-Deplyoment für eine WordPress-Anwendung dargestellt. [4]

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress-mysql
spec:
  replicas: 1
  template:
    spec:
      containers:
        - image: mysql:5.6
          name: mysql
          ports:
            - containerPort: 3306
              name: mysql
          volumeMounts:
            - name: mysql-persistent-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-persistent-storage
          persistentVolumeClaim:
            claimName: mysql-pv-claim
```

Stürzt einmal der Pod mit der Datenbank ab, so wird diese in kurzer Zeit neu gestartet, dafür sorgt der Replication Controller, aber es gibt eine Downtime. Die Daten sind allerdings über das externe Volume sicher.

Mit einem `deployment` kann man aufgrund des Replication Controllers auch mehrere Pods gleichen Typs über `replicas: n` im Skript spezifizieren. Das bedeutet allerdings nicht gleich, dass n Duplikate von Datenbanken in n Pods auch miteinander kommunizieren. Kubernetes würde lediglich beim Einsatz einer Service-Ressource als vereinfacht ausgedrückt interner DNS und LoadBalancer für alle Datenbanken-Pods fungieren und eingehenden Traffic auf die n-Pods verteilen. Dieser Fall ist i.A. nicht für den Betrieb von Datenbanken zu empfehlen, denn gemäß CAP-Theorem steigt dadurch zwar die Verfügbarkeit bei steigendem Traffic, die Konsistenz wird bei einer Vielzahl von Schreibvorgängen aber nicht mehr gewährleistet sein. Stateless Applications können im Gegensatz zu Datenbanken als Stateful Applications so wunderbar betrieben und von Kubernetes automatisch lastverteilt werden.

Je nach Art der zu betreibenden Anwendung ergeben sich evtl. auch Einsatzmöglichkeiten für Datenbanken. Ein Beispiel wäre ein Datenbestand in einem Volume, der nur von einer Admin-Seite über ein separates Datenbank-Deployment schreibend verändert werden kann und über einen parallel dazu laufendes `deployment` aus mehreren Datenbanken nur gelesen werden darf. So eine Trennung führt zwar auch zu einer verminderten Konsistenz, aber auch zur einer hohen Verfügbarkeit. Aus administrativer Sicht ist dieser Fall natürlich auch mit mehr manuellem Aufwand verbunden. Um einen generellen Einsatz von Stateful Applikationen in Kubernetes zu vereinfachen wurden die Stateful Sets eingeführt.

### Stateful Sets

- komplexe und ausfallsicherer stateful = StatefulSets
- für Stateful Applicationen gedacht
- Unterschiede in der Bennennung und Handhabung der Container
- Beispiel 1: Operator bei MongoDB
- Beispiel 2: Cassandra oder ähnliches

| #   | Literatur                                                                                                                                                                                                                                                |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [1] | **Thomas Fricke (16.01.2018)**: _Kubernetes: Architektur und Einsatz – Eine Einführung mit Beispielen_, https://www.informatik-aktuell.de/entwicklung/methoden/kubernetes-architektur-und-einsatz-einfuehrung-mit-beispielen.html, aufgerufen 02.12.2019 |
| [2] | _Nichtflüchtige Speicher mit WordPress und MySQL verwenden_, https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk?hl=de, aufgerufen 01.11.2019                                                                                      |
| [3] | **Nicolas Bonorquez**: _Yes, You Can: Deploying Databases on Kubernetes with StatefulSets_, https://sweetcode.io/deploying-databases-kubernetes-statefulsets/, aufgerufen 28.11.2019                                                                     |
| [4] | _Deployments_, https://kubernetes.io/docs/concepts/workloads/controllers/deployment/, aufgerufen 12.1.2020                                                                                                                                               |
| [5] | _Example: Deploying WordPress and MySQL with Persistent Volumes_, https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/, aufgerufen 12.1.2020                                                                     |

---

[<< Kubernetes](3_k8s.md) | [Inhaltsverzeichnis](0_title.md) | [Fazit >>](5_fazit.md)

---
