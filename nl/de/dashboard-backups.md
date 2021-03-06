---

copyright:
  years: 2017,2018
lastupdated: "2017-10-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Sicherungen
{: #backups}

Sie können Sicherungen erstellen und über die Registerkarte _Sicherungen_ der Seite _Verwalten_ Ihres Service-Dashboards herunterladen. Dabei sind tägliche, wöchentliche und monatliche Sicherungen sowie Sicherungen nach Bedarf verfügbar. Die Aufbewahrungszeit richtet sich nach folgendem Zeitplan:

Sicherungstyp|Aufbewahrungszeitplan
----------|-----------
Täglich|Tägliche Sicherungen werden 7 Tage aufbewahrt
Wöchentlich|Wöchentliche Sicherungen werden 4 Wochen aufbewahrt
Monatlich|Monatliche Sicherungen werden 3 Monate aufbewahrt
Bei Bedarf|Es wird eine bei Bedarf erstellte Sicherung aufbewahrt. Dabei ist die aufbewahrte Sicherung immer die letzte bei Bedarf erstellte Sicherung.
{: caption="Tabelle 1. Aufbewahrungszeitplan für Sicherungen" caption-side="top"}

Sicherungszeitpläne und Aufbewahrungsrichtlinien sind festgelegt. Falls Sie mehr Sicherungen benötigen als die, die im Aufbewahrungszeitplan vorgesehen sind, sollten Sie Sicherungen herunterladen und Sicherungsarchive gemäß Ihren Geschäftsanforderungen aufbewahren.

## Vorhandene Sicherungen anzeigen

Tägliche Sicherungen Ihrer Datenbank werden automatisch geplant. Navigieren Sie zum Anzeigen Ihrer vorhandenen Sicherungen zu der Seite *Verwalten* Ihres Service-Dashboards. 

  ![Sicherungen](./images/scylla-backups-show.png "Liste der Sicherungen im Service-Dashboard")

Klicken Sie auf eine Zeile, um die Optionen für die entsprechende verfügbare Sicherung zu erweitern.

  ![Sicherungsoptionen](./images/scylla-backups-options.png "Optionen für eine Sicherung.") 

### API verwenden, um vorhandene Backups anzuzeigen

Eine Liste der Backups steht am Endpunkt `GET /2016-07/deployments/:id/backups` zur Verfügung. Sowohl der Basisendpunkt als auch die Serviceinstanz-ID wird in der _Übersicht_ des Service angezeigt. Beispiel: 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```    

## Sicherung bedarfsgerecht erstellen

Neben geplanten Sicherungen können Sie manuelle Sicherungen erstellen. Navigieren Sie zum Erstellen einer manuellen Sicherung zu der Seite *Verwalten* Ihres Service-Dashboards und klicken Sie auf *Jetzt sichern*.

### API verwenden, um eine Sicherung zu erstellen

Senden Sie eine POST-Anforderung an den Endpunkt der Sicherung, um eine manuelle Sicherung zu starten: `POST /2016-07/deployments/:id/backups`. Daraufhin wird sofort die Anleitungs-ID mit Informationen zur aktiven Sicherung zurückgegeben. Sie müssen den Endpunkt der Sicherung überprüfen, um festzustellen, ob die Sicherung fertiggestellt wurde und um die zugehörige Sicherungs-ID (backup_id) zu suchen, bevor Sie sie verwenden können. Verwenden Sie `GET /2016-07/deployments/:id/backups/`.

## Sicherung wiederherstellen
Gehen Sie zum Wiederherstellen einer Sicherung in einer neuen Serviceinstanz wie folgt vor:

1. Führen Sie die Schritte zum Anzeigen der vorhandenen Sicherungen aus.
2. Klicken Sie dann in die entsprechende Zeile, um die Optionen für die Sicherung zu erweitern, die Sie herunterladen wollen.
3. Klicken Sie auf die Schaltfläche **Wiederherstellen**. Es wird eine Nachricht darüber angezeigt, dass eine Wiederherstellung eingeleitet wurde. Die neue Serviceinstanz erhält automatisch den Namen "scylla-restore-[timestamp]" und wird beim Start der Bereitstellung in Ihrem Dashboard angezeigt.

### Wiederherstellung über die {{site.data.keyword.cloud_notm}}-CLI

Führen Sie die folgenden Schritte aus, um mithilfe der {{site.data.keyword.cloud_notm}}-CLI eine Sicherung aus einem aktiven Scylla-Service in einem neuen Scylla-Service wiederherzustellen.

1. Sie müssen die CLI [herunterladen und installieren](https://console.{DomainName}/docs/cli/index.html#overview). 
2. Suchen Sie auf der Seite _Sicherungen_ in Ihrem Service die Sicherung aus, die Sie wiederherstellen möchten, und kopieren Sie die Sicherungs-ID.  
  **ODER**  
  Verwenden Sie `GET /2016-07/deployments/:id/backups`, um eine Sicherung und die zugehörige ID über die Compose-API zu suchen. Sowohl der Basisendpunkt als auch die Serviceinstanz-ID wird in der _Übersicht_ des Service angezeigt. Beispiel: 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  Die Antwort beinhaltet eine Liste aller verfügbaren Sicherungen für diese Serviceinstanz. Wählen Sie die Sicherung aus, die für die Wiederherstellung verwendet werden soll, und kopieren Sie die zugehörige ID.

3. Melden Sie sich mit dem entsprechenden Konto und den zugehörigen Berechtigungsnachweisen an. Verwenden Sie hierfür den Befehl `ibmcloud login` (oder `ibmcloud login -help`, damit alle Anmeldeoptionen angezeigt werden).

4. Wechseln Sie mit `ibmcloud target -o "$YOUR_ORG" -s "YOUR_SPACE"` zu Ihrer Organisation und Ihrem Bereich.

5. Verwenden Sie den Befehl `service create`, um einen neuen Bereich bereitzustellen und stellen Sie den Quellenservice und die spezifische Sicherung zur Verfügung, die Sie in einem JSON-Objekt wiederherstellen. Beispiel:
``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  Verwenden Sie für das Feld _SERVICE_ die Angabe 'compose-for-scylladb' und wählen Sie für das Feld _PLAN_ je nach der verwendeten Umgebung entweder 'Standard' oder 'Enterprise' aus. In _SERVICE\_INSTANCE\_NAME_ geben Sie den Namen für Ihren neuen Service an. _source\_service\_instance\_id_ ist die Serviceinstanz-ID der Quelle der Sicherung. Diese kann durch Ausführen von `ibmcloud cf service DISPLAY_NAME --guid` abgerufen werden, wobei _DISPLAY\_NAME_ der Name des Scylla-Service ist, von dem die Sicherung stammt. 
  
  Enterprise-Benutzer müssen ferner angeben, welcher Cluster in dem JSON-Objekt mit dem Parameter `"cluster_id": "$CLUSTER_ID"` bereitgestellt werden soll.
  
### Migration auf eine neue Version

Einige Upgrades der übergeordneten Version sind in der aktuell ausgeführten Bereitstellung nicht verfügbar. Sie müssen einen neuen Service bereitstellen, auf dem die per Upgrade aktualisierte Version ausgeführt wird, und anschließend Ihre Daten mit einer Sicherung in diesen neuen Service migrieren. Dieser Prozess entspricht der [Wiederherstellung einer Sicherung](#restoring-a-backup), jedoch mit dem Unterschied, dass Sie die Version angeben, auf die Sie das Upgrade durchführen wollen.

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

Die Wiederherstellung einer älteren Version eines {{site.data.keyword.composeForScyllaDB}}-Service auf einem neuen Service unter Ausführung von Scylla 2.0.3 sieht folgendermaßen aus:
```
ibmcloud service create compose-for-scylladb Standard migrated_scylla -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"2.0.3"  }'

