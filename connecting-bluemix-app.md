---

copyright:
  years: 2016,2018
lastupdated: "2018-05-09"

subcollection: compose-for-scylladb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an {{site.data.keyword.cloud_notm}} application
{: #ibmcloud-cf-app}

To connect an {{site.data.keyword.cloud}} application to your service, use the service credentials that are created when the service is provisioned.

## Connecting with the 'Hello World' sample app

The [sample app](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) demonstrates how to use Node.js to connect to a {{site.data.keyword.composeForScyllaDB}} service by using the provided credentials. The application creates, reads from, and writes to a database.

Download the sample app and follow the instructions in the readme file. Then, in your application details page in {{site.data.keyword.cloud_notm}}, click **View APP** to view the contents of the *examples* table.

## Available credentials

Field Name|Description
----------|-----------
`db_type`|The type of database that is offered by the service, in this case `scylla`.
`uri_cli_1`|A secondary `cqlsh` shell command line that connects to the database instance.
`maps`|A ScyllaDB connection map that provides the information that is needed by various drivers to associate internal IP addresses with the external DNS names of a ScyllaDB database.
`name`|The database deployment name.
`uri_cli`|A `cqlsh` shell command line that connects to the database instance.
`uri_direct_2`|A third URI that can be used to connect to the service. The format is the same as `uri`.
`uri_direct_1`|A secondary URI that can be used to connect to the service. The format is the same as `uri`.
`ca_certificate_base64` `(optional)`|A base64 encoded, self-signed certificate that is used to confirm that an application is connecting to the appropriate server. The certificate is only present on services that have a self-signed instead of a Let's Encrypt certificate. You need to decode the key before you can use it, as shown in the sample application.
`deployment_id`|An internal identifier for the service as created within Compose.
`uri_cli_2`|A third `cqlsh` shell command line that connects to the database instance.
`uri`|The URI that is used when connecting to the service, which includes the schema (`scylla:`), password, host name of server, port number to connect to, and database name.
{: caption="Table 1. Compose for ScyllaDB credentials" caption-side="top"}

**Note:** Three `haproxy` portals provide access to the Scylla service. You can use any of `uri`, `uri_direct_1`, and `uri_direct_2` to connect. In your applications, switch between `uri`, `uri_direct_1`, and `uri_direct_2` to manage responses to connection failures.
