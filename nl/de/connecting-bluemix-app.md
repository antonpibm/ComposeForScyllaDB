---

copyright:
  years: 2016,2018
lastupdated: "2018-05-09"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.cloud_notm}}-Anwendung verbinden

Verbinden Sie eine {{site.data.keyword.cloud}}-Anwendung mit Ihrem Service mithilfe der Berechtigungsnachweise dieses Service, die bei dessen Bereitstellung erstellt werden.

## Verbindungsherstellung mit der Beispielapp 'Hello World'

Die Beispielapp [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) veranschaulicht die Verwendung von Node.js zur Herstellung einer Verbindung zu einem {{site.data.keyword.composeForScyllaDB}}-Service mithilfe der bereitgestellten Berechtigungsnachweise. Die Anwendung erstellt eine Datenbank, liest daraus und schreibt darin.

Laden Sie die Beispielapp herunter und befolgen Sie die Anweisungen in der Readme-Datei. Anschließend klicken Sie auf der Detailseite für die Anwendung in {{site.data.keyword.cloud_notm}} auf **App anzeigen**, um den Inhalt der Tabelle *examples* anzuzeigen.

## Verfügbare Berechtigungsnachweise

Feldname|Beschreibung
----------|-----------
`db_type`|Der Datenbanktyp, der vom Service angeboten wird, in diesem Fall `scylla`.
`uri_cli_1`|Eine sekundäre `cqlsh`-Shellbefehlszeile, die eine Verbindung zur Datenbankinstanz herstellt.
`maps`|Eine ScyllaDB-Verbindungskarte, die die Informationen bereitstellt, die verschiedene Treiber zur Zuordnung interner IP-Adressen zu externen DNS-Namen einer ScyllaDB-Datenbank benötigen.
`name`|Der Name der Datenbankimplementierung.
`uri_cli`|Eine `cqlsh`-Shellbefehlszeile, die eine Verbindung zur Datenbankinstanz herstellt.
`uri_direct_2`|Ein dritter URI, der für die Verbindungsherstellung zum Service verwendet werden kann. Das Format ist dasselbe wie für `uri`.
`uri_direct_1`|Ein sekundärer URI, der für die Verbindungsherstellung zum Service verwendet werden kann. Das Format ist dasselbe wie für `uri`.
`ca_certificate_base64` `(optional)`|Ein selbst signiertes Zertifikat in Base64-Codierung, mit dem bestätigt wird, dass eine Anwendung eine Verbindung zum entsprechenden Server herstellt. Das Zertifikat ist nur für Services vorhanden, die ein selbst signiertes Zertifikat anstelle eines Zertifikats von Let's Encrypt verwenden. Der Schlüssel muss vor seiner Verwendung decodiert werden, wie in der Beispielanwendung gezeigt.
`deployment_id`|Eine interne ID für den Service entsprechend der Erstellung in Compose.
`uri_cli_2`|Eine dritte `cqlsh`-Shellbefehlszeile, die eine Verbindung zur Datenbankinstanz herstellt.
`uri`|Der bei der Herstellung einer Verbindung zum Service verwendete URI, der Folgendes enthält: Schema (`scylla:`), Kennwort, Hostname des Servers, Portnummer für die Verbindungsherstellung und Datenbankname.
{: caption="Tabelle 1. Compose for ScyllaDB - Berechtigungsnachweise" caption-side="top"}

**Hinweis:** Zwei `haproxy`-Portale bieten Zugriff auf den Scylla-Service. Sowohl `uri` als auch `uri_direct_1` wie auch `uri_direct_2` können zum Herstellen der Verbindung verwendet werden. In Ihrer Anwendung wechseln Sie zwischen `uri`, `uri_direct_1` und `uri_direct_2`, um die Reaktionen auf Verbindungsfehler zu verwalten.
