---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}

# Lernprogramm 'Einführung'

* {: download} Herzlichen Glückwunsch! Sie haben die Hello World-Beispielanwendung unter {{site.data.keyword.Bluemix}} bereitgestellt! Befolgen Sie diesen schrittweisen Leitfaden, um zu starten. Oder laden Sie den <a class="xref" href="http://bluemix.net" target="_blank" title="(Beispielcode herunterladen)"><img class="hidden" src="../../images/btn_starter-code.svg" alt="Anwendungscode herunterladen" />Beispielcode herunter</a> und beginnen Sie auf eigene Faust.

Wenn Sie diesem Lernprogramm folgen, werden Sie eine Entwicklungsumgebung einrichten, eine App lokal und unter {{site.data.keyword.Bluemix}} bereitstellen und einen {{site.data.keyword.Bluemix}}-Datenbankservice in Ihre App integrieren.

## Vorbemerkungen
{: #prereqs}
* [Git ![Symbol 'Externer Link'](../../icons/launch-glyph.svg "Symbol 'Externer Link'")](https://git-scm.com/downloads){: new_window}
* [Cloud Foundry-CLI ![Symbol 'Externer Link'](../../icons/launch-glyph.svg "Symbol 'Externer Link'")](https://github.com/cloudfoundry/cli#downloads){: new_window}
* [Swift-Compiler ![Symbol 'Externer Link'](../../icons/launch-glyph.svg "Symbol 'Externer Link'")](https://swift.org/download/) für Ihre Plattform.

## Schritt 1: Klonen Sie die Beispielapp.
{: #clone}

Jetzt können Sie beginnen, mit der einfachen Swift-App zu arbeiten. Klonen Sie das Repository und wechseln Sie in das Verzeichnis, in dem sich die Beispielapp befindet. 
  ```
git clone https://github.com/IBM-Bluemix/get-started-swift
  ```
  {: pre}

  ```
cd get-started-swift
  ```
  {: pre}

  Lesen Sie die Dateien im Verzeichnis *get-started-swift*, um sich mit deren Inhalt vertraut zu machen. 

## Schritt 2: Führen Sie die App lokal aus.
{: #run_locally}

Sobald Sie den Swift-Compiler installiert und dieses Git-Repository geklont haben, können Sie die Anwendung kompilieren und ausführen. Wechseln Sie in das Stammverzeichnis dieses Repositorys in Ihrem System und geben Sie den folgenden Befehl aus: 
```
swift build
```
{: pre}

Die Ausführung dieses Befehls kann einige Minuten dauern. 

Sobald die Anwendung erfolgreich kompiliert wurde, können Sie die ausführbare Datei, die vom Swift-Compiler generiert wurde, ausführen. 
```
.build/debug/kitura-helloworld
```
{: pre}

Sie sollten eine Ausgabe ähnlich der folgenden sehen: 

```
Server is listening on port: 8080
```
{: screen}

Ihre App finden Sie unter http://localhost:8080.

## Schritt 3: Bereiten Sie die App für die Bereitstellung vor.
{: #prepare}

Für die Bereitstellung unter {{site.data.keyword.Bluemix_notm}} kann es hilfreich sein, die Datei 'manifest.yml' zu installieren. Die Datei 'manifest.yml' enthält Basisinformationen zu Ihrer App wie den Namen, wieviel Speicher für jede Instanz zugeordnet werden soll und die Route. Wir haben eine Beispieldatei 'manifest.yml' im Verzeichnis `get-started-swift` bereitgestellt. 

Öffnen Sie die Datei 'manifest.yml' und ändern Sie die Angabe für `name` von `GetStartedSwift` in den Namen Ihrer App <var class="keyword varname" data-hd-keyref="app_name">app_name</var>.
{: download}

  ```
  applications:
   - name: GetStartedSwift
     random-route: true
     memory: 256M
     command: kitura-helloworld
     buildpack: swift_buildpack
  ```
  {: codeblock}

In dieser Datei 'manifest.yml' generiert **random-route: true** eine zufällige Route für Ihre App, um zu verhindern, dass Ihre Route mit anderen Routen kollidiert. Wenn Sie möchten, können Sie **random-route: true** durch **host: myChosenHostName** ersetzen und einen Hostnamen Ihrer Wahl angeben. [Weitere Informationen...](/docs/manageapps/depapps.html#appmanifest)
{: tip}

## Schritt 4: Stellen Sie die App bereit.
{: #deploy}

Sie können die Cloud Foundry-CLI verwenden, um Apps bereitzustellen.

Wählen Sie Ihren API-Endpunkt aus.
  ```
cf api <API-endpoint>
  ```
  {: pre}

Ersetzen Sie *API-endpoint* im Befehl durch einen API-Endpunkt aus der folgenden Liste: 

|Bereich         |API-Endpunkt|
|:---------------|:-------------------------------|
| USA (Süden)    | https://api.ng.bluemix.net|
| Großbritannien | https://api.eu-gb.bluemix.net|
| Sydney         | https://api.au-syd.bluemix.net|
| Frankfurt      | https://api.eu-de.bluemix.net | 

Melden Sie sich bei Ihrem {{site.data.keyword.Bluemix_notm}}-Konto an.

   ```
cf login
```
   {: pre}
   
Wenn Sie sich nicht über den Befehl `cf login` oder `bx login` anmelden können, weil Sie über eine eingebundene Benutzer-ID verfügen, verwenden Sie entweder den Befehl `cf login --sso` oder den Befehl `bx login --sso`, um sich mit Ihrer Single-Sign-on-ID anzumelden. Weitere Informationen finden Sie unter [Mit eingebundener ID anmelden](https://console.bluemix.net/docs/cli/login_federated_id.html#federated_id).

Übertragen Sie Ihre App aus dem Verzeichnis *get-started-swift* mit einer Push-Operation an {{site.data.keyword.Bluemix_notm}}.
   ```
cf push
```
   {: pre}

Dieser Vorgang kann einige Minuten dauern. Falls ein Fehler im Bereitstellungsprozess auftritt, können Sie mithilfe des Befehls `cf logs <Your-App-Name> --recent` nach dem Fehler suchen.

Wenn die Bereitstellung abgeschlossen ist, sollten Sie eine Nachricht sehen, die anzeigt, dass Ihre App ausgeführt wird. Ihre App wird an der URL angezeigt, die in der Ausgabe der Push-Operation aufgelistet ist. Sie können auch den Befehl `cf apps` ausgeben, um Ihren Appstatus und die URL anzuzeigen. 

## Schritt 5: Fügen Sie eine Datenbank hinzu.
{: #add_database}

Als nächstes werden wir eine NoSQL-Datenbank zu dieser Anwendung hinzufügen und die Anwendung so einrichten, dass sie lokal und unter {{site.data.keyword.Bluemix_notm}} ausgeführt werden kann.

1. Melden Sie sich in Ihrem Browser bei {{site.data.keyword.Bluemix_notm}} an. Navigieren Sie zum `Dashboard`. Wählen Sie Ihre Anwendung durch Klicken auf den zugehörigen Namen in der Spalte `Name` aus. 
2. Klicken Sie auf `Verbindungen` und dann auf `Neuen verbinden`.
3. Wählen Sie im Abschnitt `Data &  Analytics` den Eintrag `Cloudant NoSQL DB` aus.
4. Wählen Sie einen Preistarif aus. Bluemix bietet kostenlose `Lite`-Tarife für eine augewählte Gruppe seiner Cloud Services mit ausreichender Kapazität für den Start. 
5. Wählen Sie `Erneutes Staging` aus, wenn Sie dazu aufgefordert werden. {{site.data.keyword.Bluemix_notm}} startet Ihre Anwendung erneut und bietet die Datenbankberechtigungsnachweise für Ihre Anwendung unter Verwendung der Umgebungsvariablen `VCAP_SERVICES`. Diese Umgebungsvariable ist nur dann für die Anwendung verfügbar, wenn sie unter {{site.data.keyword.Bluemix_notm}} ausgeführt wird.

Umgebungsvariablen ermöglichen es Ihnen, die Bereitstellungseinstellungen von Ihrem Quellcode zu trennen. Anstelle der festen Codierung eines Datenbankkennworts können Sie dieses in einer Umgebungsvariablen speichern, auf die Sie in Ihrem Quellcode verweisen. [Weitere Informationen...](/docs/manageapps/depapps.html#app_env)
{: tip}

## Schritt 6: Verwenden Sie die Datenbank.
{: #use_database}

Wir werden jetzt Ihren lokalen Code aktualisieren, um auf diese Datenbank zu verweisen. Erstellen Sie eine json-Datei, die die Berechtigungsnachweise für die Services speichert, die die Anwendung verwendet. Diese Datei wird NUR dann verwendet, wenn die Anwendung lokal ausgeführt wird. Bei der Ausführung in {{site.data.keyword.Bluemix_notm}} werden die Berechtigungsnachweise aus der Umgebungsvariablen VCAP_SERVICES gelesen. 

Erstellen Sie eine Datei mit dem Namen `config.json` im Verzeichnis `Sources` mit dem folgenden Inhalt (siehe Beispiel: config.json.example):
 ```
 {
    "vcap":{
       "services":{
          "cloudantNoSQLDB":[
             {
                "credentials":{
                   "host":"<host>",
                   "password":"<password>",
                   "port":443,
                   "url":"<url>",
                   "username":"<username>"
                },
                "label":"cloudantNoSQLDB",
                "name": "CloudantService"
             }
          ]
       }
    }
 }
 ```
{: pre}

Diese Beispielanwendung verwendet das Paket Swift-cfenv für die Interaktion mit Bluemix, um ein Parsing für Umgebungsvariablen durchzuführen. [Weitere Informationen...](https://packagecatalog.com/package/IBM-Swift/Swift-cfenv)
Kehren Sie in die Benutzerschnittstelle von {{site.data.keyword.Bluemix_notm}} zurück und wählen Sie Ihre App -> Verbindungen -> Cloudant -> Berechtigungsnachweise anzeigen aus. 

Kopieren Sie einfach die Berechtigungsnachweise und fügen Sie diese in die entsprechendne Felder in Ihrer lokalen Datei 'config.json' ein. 

Erstellen Sie einen Build und führen Sie Ihre Anwendung lokal aus. 
 ```
swift build  
 ```
 {: pre}

  ```
.build/debug/kitura-helloworld
 ```
 {: pre}

Ihre App finden Sie unter: http://localhost:8080. Alle Namen, die Sie in die App eingegeben haben, werden jetzt zur Datenbank hinzugefügt. Diese Beispielanwendung verwendet das Kitura-CouchDB-Paket für die Interaktion mit Cloudant. [Weitere Informationen...](https://packagecatalog.com/package/IBM-Swift/Kitura-CouchDB)
{: tip}

Ihre lokale App und  die {{site.data.keyword.Bluemix_notm}}-App verwenden die Datenbank gemeinsam. Ihre {{site.data.keyword.Bluemix_notm}}-App wird an der URL angezeigt, die in der Ausgabe der oben erwähnten Push-Operation aufgelistet ist. Namen, die Sie in einer der Apps eingeben, sollten nach einer Aktualisierung des Browsers in beiden angezeigt werden. Denken Sie daran, dass Sie Ihre App stoppen, wenn Sie nicht benötigt wird, damit Ihnen nicht unerwartete Gebühren belastet werden.
{: tip}
