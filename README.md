# linuxmuster-apt-cacher-ng

apt-cacher-ng Docker App für linuxmuster. 

TESTVERSION - **NICHT PRODUKTIV VERWENDEN**

## Verwendung

Entweder du hast schon einen Dockerhost mit docker, docker-compose,
dehydrated und nginx oder du erzeugst dir kurz einen Dockerhost mit
create-docker-host:
https://github.com/linuxmuster-ext-docker/create-docker-host

### Dienstenamen und Zertifikat

Für apt benötigst du keinen eigenen Dienstenamen oder Zertifikat, weil
der Proxy nur auf http lauscht und cached.

### App herunterladen, konfigurieren und starten

* Klone dieses Repo auf deinen Dockerhost nach ``/srv/docker``
  * ``cd /srv/docker``
  * ``git clone https://github.com/jolly-jump/linuxmuster-apt-cacher-ng``
* Wechsle in das App-Verzeichnis: ``cd linuxmuster-apt-cacher-ng``
* Passe die Werte in der Datei ``apt-cacher-ng.ini`` an.
* Erzeuge eine Konfiguration mit: ``./deploy/bin/turnkey -c apt-cacher-ng.ini``
* Starte die App mit dem Befehl ``docker-compose up -d``

### Proxy in Debian/Ubuntu-Clients einrichten

In einem Ubuntu/Debian-basierten System kann man nun den Proxy in der
``/etc/apt/apt.conf.d/00proxy`` eintragen, z.B. so, wobei hier
`172.16.17.2` die IP-Adresse des Dockerhosts ist.

```
Acquire::http { Proxy "http://172.16.17.2:3142"; };
Acquire::https { Proxy "https://"; };
```
Wer bei obiger Konfiguration eine Ausnahme braucht, kann diese so definieren: 
```
Acquire::http::Proxy { 
    packages.gitlab.com DIRECT;
    zweite.ausnahme.de DIRECT;
}
```

Wer nur manche Repositories cachen will, nicht aber generell, der kann direkt in ``/etc/apt/sources.list`` und ähnlichen Dateien direkt den Proxy eintragen:
```
deb http://172.16.17.2:3142/ftp.debian.org/debian stable main contrib non-free
```

