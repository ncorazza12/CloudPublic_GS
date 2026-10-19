# Laboratório Azure: lab-popovici-01 (CP2)

Evidências da execução do laboratório **lab-popovici-01**, uma arquitetura Azure baseada em Container Apps, ACR, Managed Identity, VNet, Private DNS, PostgreSQL Flexible Server, Key Vault, Storage Account, Application Insights, Log Analytics e Azure Communication Services.

- **Assinatura:** Azure for Students
- **Região:** Brazil South (`brazilsouth`)
- **Resource Group:** `rg-lab-popovici-01`
- **Ferramentas:** Azure Cloud Shell (PowerShell) e Azure CLI

> Esta é a primeira entrega (execução do laboratório). A documentação completa do ambiente e o vídeo explicativo serão adicionados na segunda entrega.

## Evidências

### Print 1: Registro dos Resource Providers

Verificação do estado de registro dos providers necessários ao laboratório. `Microsoft.App` e `Microsoft.Communication` constam como `Registered`.

![Print 1: Registro dos Resource Providers](Print%201%20Registro%20dos%20Resource%20Providers.jpeg)

### Print 2: Extensões do Azure CLI

Listagem das extensões instaladas: `containerapp` (preview), `communication` e `application-insights`, necessárias para os recursos do laboratório.

![Print 2: Extensões do Azure CLI](Print%202%20Extens%C3%B5es%20do%20Azure%20CLI.jpeg)

### Print 3: Seleção da região e assinatura

Consulta de disponibilidade do Container Apps e do SKU `Standard_B1ms` do PostgreSQL. O B1ms foi listado em `brazilsouth`, e não em `eastus` e `eastus2`. Essa evidência justifica o desvio do roteiro, que previa `eastus`. A assinatura utilizada é Azure for Students.

![Print 3: Seleção da região e assinatura](Print%203%20Sele%C3%A7%C3%A3o%20da%20regi%C3%A3o%20e%20assinatura.jpeg)

### Print 4: Resource Group

Criação e validação do `rg-lab-popovici-01` em `brazilsouth`, com provisionamento `Succeeded`.

![Print 4: Resource Group](Print%204%20Resource%20Group.jpeg)

### Print 5: Sub-redes e link da Private DNS Zone

A VNet possui `snet-containerapps` (10.20.0.0/23) e `snet-postgresql` (10.20.2.0/24), esta delegada a `Microsoft.DBforPostgreSQL/flexibleServers`. O link entre a zona DNS privada e a VNet está `Completed`/`Succeeded`.

![Print 5: Sub-redes e link da Private DNS Zone](Print%205%20Sub-redes%20e%20link%20da%20Private%20DNS%20Zone.jpeg)

### Print 6: Resource Group no Portal

Visão geral do `rg-lab-popovici-01` com a VNet e a Private DNS Zone criadas, em Brazil South e Global, respectivamente.

![Print 6: Resource Group no Portal](Print%206%20Resource%20Group%20no%20Portal.jpeg)

### Print 7: Disponibilidade dos nomes globais

Verificação prévia dos nomes do ACR e do Storage Account, ambos com `nameAvailable: true`. Evita conflito de nomes antes da criação.

![Print 7: Disponibilidade dos nomes globais](Print%207%20Disponibilidade%20dos%20nomes%20globais.jpeg)

### Print 8: PostgreSQL Flexible Server

Servidor `lab-popovici-01-pg-4827` em estado `Ready`, versão 16, SKU `Standard_B1ms` (Burstable), com acesso público `Disabled`, ou seja, acesso privado.

![Print 8: PostgreSQL Flexible Server](Print%208%20PostgreSQL%20Flexible%20Server.jpeg)

### Print 9: Banco labdb e registro DNS privado

O banco `labdb` foi criado no servidor. Na zona DNS privada, o serviço registrou automaticamente o IP privado do servidor (10.20.2.4), dentro da subnet `snet-postgresql`.

![Print 9: Banco labdb e registro DNS privado](Print%209%20Banco%20labdb%20e%20registro%20DNS%20privado.jpeg)

### Print 10: Rede do PostgreSQL no Portal

O método de conectividade é Acesso privado (Integração VNet), usando `lab-popovici-01-vnet/snet-postgresql` e a zona DNS privada `lab-popovici-01-4827.postgres.database.azure.com`.

![Print 10: Rede do PostgreSQL no Portal](Print%2010%20Rede%20do%20PostgreSQL%20no%20Portal.jpeg)

### Print 11: Observabilidade

Log Analytics `lab-popovici-01-logs` e Application Insights `lab-popovici-01-insights` em `Succeeded`, com o Application Insights vinculado ao workspace.

![Print 11: Observabilidade](Print%2011%20Observabilidade.jpeg)

### Print 12: Managed Identities

Criação das identidades gerenciadas `lab-popovici-01-identity` (aplicação) e `lab-popovici-01-migration` (job), separadas para aplicar o princípio do menor privilégio.

![Print 12: Managed Identities](Print%2012%20Managed%20Identities.jpeg)

### Print 13: Azure Container Registry

ACR `labpopovici01acr4827`, SKU Basic, com usuário administrador desabilitado. O repositório `lab-app` contém a tag `v1`, importada da imagem de exemplo da Microsoft.

![Print 13: Azure Container Registry](Print%2013%20Azure%20Container%20Registry.jpeg)

### Print 14: Key Vault

Cofre `lab-popovici-01-kv-4827` com autorização RBAC. Ao usuário foi atribuída a função `Key Vault Secrets Officer`, permitindo criar o segredo `LabSecret`. O valor do segredo não é exibido.

![Print 14: Key Vault](Print%2014%20Key%20Vault.jpeg)

### Print 15: Storage Account

Conta `labpopovici01st4827` (Standard_LRS, StorageV2) com TLS mínimo 1.2 e acesso público anônimo a blobs desabilitado. O container `appfiles` foi criado com autenticação via login.

![Print 15: Storage Account](Print%2015%20Storage%20Account.jpeg)

### Print 16: Permissões RBAC das identidades

A identidade da aplicação possui `Key Vault Secrets User`, `Storage Blob Data Reader` e `AcrPull`, cada uma no escopo do recurso correspondente. A identidade do job possui apenas `AcrPull` no ACR.

![Print 16: Permissões RBAC das identidades](Print%2016%20Permiss%C3%B5es%20RBAC%20das%20identidades.jpeg)

### Print 17: Container Apps Environment

A `snet-containerapps` foi delegada a `Microsoft.App/environments`. O ambiente `lab-popovici-01-env` está `Succeeded`, em Brazil South, integrado a essa subnet e no modo Workload Profiles. Essa delegação é um desvio do roteiro.

![Print 17: Container Apps Environment](Print%2017%20Container%20Apps%20Environment.jpeg)

### Print 18: Container App

O app `lab-popovici-01-app` está `Succeeded`, usando a imagem `labpopovici01acr4827.azurecr.io/lab-app:v1`. A revisão ativa está `Healthy`. O pull da imagem usa identidade gerenciada, sem senha.

![Print 18: Container App](Print%2018%20Container%20App.jpeg)

### Print 19: Aplicação no navegador

O endpoint HTTPS público do Container App responde com a página de exemplo "Azure Container Apps - Welcome", validando FQDN, ingress externo e pull da imagem.

![Print 19: Aplicação no navegador](Print%2019%20Aplica%C3%A7%C3%A3o%20no%20navegador.jpeg)

### Print 20: Container Apps Job

O job `lab-popovici-01-migrate` foi criado com trigger manual, identidade própria e imagem do ACR. A execução `lab-popovici-01-migrate-pthsr5w` ficou registrada no histórico e terminou com estado `Failed`. A causa não foi confirmada nos logs. A hipótese é que a imagem de exemplo é um servidor web que não termina sozinho.

![Print 20: Container Apps Job](Print%2020%20Container%20Apps%20Job.jpeg)

### Print 21: Segredo do Key Vault no Container App

O segredo `labsecret` referencia o `LabSecret` do Key Vault usando a identidade da aplicação. A variável `LAB_SECRET` aponta para `secretRef: labsecret`, e o valor do segredo permanece no Key Vault.

![Print 21: Segredo do Key Vault no Container App](Print%2021%20Segredo%20do%20Key%20Vault%20no%20Container%20App.jpeg)

### Print 22: Azure Communication Services

Communication Services, Email Communication Service e domínio `AzureManagedDomain` estão `Succeeded`, com dados em UnitedStates. O domínio gerenciado consta com DKIM, SPF e DMARC verificados.

![Print 22: Azure Communication Services](Print%2022%20Azure%20Communication%20Services.jpeg)

### Print 23: Action Group

O grupo `lab-popovici-01-alerts` está ativo, com o receptor de e-mail `LabAdmin` em status `Enabled`, destino das notificações de alertas.

![Print 23: Action Group](Print%2023%20Action%20Group.jpeg)

### Print 24: Inventário dos recursos

Listagem dos 19 recursos do Resource Group, cobrindo rede, dados, identidade, segurança, execução, observabilidade e comunicação. Inclui o Action Group `Application Insights Smart Detection`, criado automaticamente pelo Application Insights.

![Print 24: Inventário dos recursos](Print%2024%20Invent%C3%A1rio%20dos%20recursos.jpeg)

### Print 25: Resource Visualizer

Diagrama dos recursos do `rg-lab-popovici-01` no Portal, mostrando as relações entre eles.

![Print 25: Resource Visualizer](Print%2025%20Resource%20Visualizer.jpeg)
