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

# Backups
{: #backups}

É possível criar e fazer download de backups na guia _Backups_ da página _Gerenciar_ de seu painel de serviço. Backups diários, mensais e on demand estão disponíveis. Eles são retidos de acordo com planejamento a seguir:

Tipo de backup|Planejamento de retenção
----------|-----------
Diárias|Os backups diários são retidos por 7 dias
Semanal|Backups semanais são retidos por 4 semanas
Mensal|Backups mensais são retidos por 3 meses
On demand|Um backup on demand é retido. O backup retido é sempre o backup on demand mais recente.
{: caption="Tabela 1. Planejamento de retenção de backup" caption-side="top"}

Planejamentos de backup e políticas de retenção são fixos. Se você precisar manter mais backups do que o planejamento de retenção permite, faça download de backups e retenha os arquivos de acordo com as suas necessidades de negócios.

## Visualizando backups existentes

Os backups diários de seu banco de dados são planejados automaticamente. Para visualizar seus backups existentes, navegue para a página *Gerenciar* de seu painel de serviço. 

  ![Backups](./images/scylla-backups-show.png "A list of backups in the service dashboard")

Clique na linha correspondente para expandir as opções de qualquer backup disponível.

  ![Backup Options](./images/scylla-backups-options.png "Options for a backup.") 

### Usando a API para visualizar backups existentes

Uma lista de backups está disponível no terminal `GET /2016-07/deployments/:id/backups`. O Terminal base e o ID da instância de serviço são ambos mostrados na _Visão geral_ do serviço. Por exemplo: 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```    

## Criando um backup sob demanda

Além de backups planejados, é possível criar um backup manualmente. Para criar um backup manual, navegue para a página *Gerenciar* de seu painel de serviço e clique em *Fazer backup agora*.

### Usando a API para criar um backup

Envie uma solicitação de POST para o terminal de backups para iniciar um backup manual: `POST /2016-07/deployments/:id/backups`. Ele retorna imediatamente com o ID da orientação e informações sobre o backup conforme ele está em execução. É necessário verificar o terminal de backups para ver se o backup foi concluído e localizar seu backup_id antes de poder utilizá-lo. Use `GET /2016-07/deployments/:id/backups/`.

## Restaurando um backup
Para restaurar um backup para uma nova instância de serviço:

1. Siga as etapas para visualizar backups existentes.
2. Clique na linha correspondente para expandir as opções do backup do qual você deseja fazer download.
3. Clique no botão **Restaurar**. Uma mensagem é exibida para permitir que você saiba que uma restauração foi iniciada. A nova instância de serviço é nomeada automaticamente "scylla-restore-[timestamp]" e aparece no painel quando o fornecimento é iniciado.

### Restaurando por meio da CLI {{site.data.keyword.cloud_notm}}

Use as etapas a seguir para usar a CLI do {{site.data.keyword.cloud_notm}} para restaurar um backup de um serviço Scylla em execução para um novo serviço Scylla.

1. Se necessário, [faça download e instale-o](https://console.{DomainName}/docs/cli/index.html#overview). 
2. Localize o backup do qual gostaria de restaurar na página _Backups_ em seu serviço e copie o ID do backup.  
  **Ou**  
  Use o `GET /2016-07/deployments/:id/backups` para localizar um backup e seu ID por meio da API do Compose. O Terminal base e o ID da instância de serviço são ambos mostrados na _Visão geral_ do serviço. Por exemplo: 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  A resposta contém uma lista de todos os backups disponíveis para essa instância de serviço. Selecione o backup do qual gostaria de restaurar e copie seu ID.

3. Efetue login com a conta e as credenciais apropriadas. `ibmcloud login` (ou `ibmcloud login -help` para ver todas as opções de login).

4. Alterne para a sua Organização e Espaço `ibmcloud target -o "$YOUR_ORG" -s "YOUR_SPACE"`

5. Use o comando `service create` para provisionar um novo serviço e forneça o serviço de origem e o backup específico que você está restaurando em um objeto JSON. Por exemplo:
``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  Para o campo _SERVICE_, use 'compose-for-scylladb' e para o campo _PLAN_, escolha Padrão ou Corporativo, dependendo de seu ambiente. _SERVICE\_INSTANCE\_NAME_ é onde você coloca o nome de seu novo serviço. O _source\_service\_instance\_id_ é o ID da instância de serviço da origem do backup; ele pode ser obtido executando `ibmcloud cf service DISPLAY_NAME --guid`, em que _DISPLAY\_NAME_ é o nome do serviço Scylla do qual o backup é feito. 
  
  Os usuários corporativos também precisam especificar qual cluster implementar no objeto JSON com o parâmetro `"cluster_id": "$CLUSTER_ID"`.
  
### Migrando para uma nova versão

Alguns upgrades de versão principal não estão disponíveis na implementação em execução atual. É necessário provisionar um novo serviço que esteja executando a versão submetida a upgrade e, em seguida, migrar seus dados para ele usando um backup. Esse processo é o mesmo que [restaurar um backup](#restoring-a-backup), exceto que você especifica a versão para a qual gostaria de fazer upgrade.

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

Por exemplo, restaurar uma versão mais antiga de um serviço {{site.data.keyword.composeForScyllaDB}} para um novo serviço executando o Scylla 2.0.3 é semelhante ao seguinte:
```
ibmcloud service create compose-for-scylladb Standard migrated_scylla -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"2.0.3"  }'

