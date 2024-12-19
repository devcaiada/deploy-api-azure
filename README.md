# Deploy de FastAPI no Azure

Este repositório contém uma aplicação simples usando o framework FastAPI, com um único endpoint `/` que retorna a mensagem "Hello, Azure!". Abaixo está o guia completo para criar e implantar essa API no Azure.

## Estrutura do Projeto

```plaintext
my_fastapi_app/
│
├── main.py              # Aplicação FastAPI
├── requirements.txt     # Dependências
└── README.md            # Documentação (este arquivo)
```

## Pré-requisitos

Antes de começar, certifique-se de ter:

1. **Python 3.9+** instalado na sua máquina.
2. **Azure CLI** instalado. Siga o [guia oficial de instalação](https://learn.microsoft.com/pt-br/cli/azure/install-azure-cli).
3. Uma conta no **Azur**e. Crie uma [aqui](https://azure.microsoft.com/pt-br/pricing/purchase-options/azure-account?icid=azurefreeaccount) se ainda não tiver.

## Guia de Deploy

### Instale as Dependências

Crie um ambiente virtual e instale as dependências necessárias:

```bash
python3 -m venv venv
source venv\Scripts\activate
pip install -r requirements.txt
```

### Teste a Aplicação Localmente

Execute a aplicação localmente para garantir que está funcionando:

```bash
uvicorn main:app --reload
```

Acesse `http://127.0.0.1:8000/` no navegador. Você verá a resposta:

```json
{
  "message": "Hello, Azure!"
}
```

### Faça Login no Azure

Faça login na sua conta Azure usando o CLI:

```bash
az login
```

Isso abrirá uma janela no navegador para você se autenticar. Após o login, retorne ao terminal.

### Crie um Grupo de Recursos

Crie um grupo de recursos no Azure para organizar seus recursos:

```bash
az group create --name meuGrupoDeRecursos --location eastus
```

### Crie um Plano de Serviço do Azure

Configure um Plano de Serviço, que define o ambiente de hospedagem e a precificação:

```bash
az appservice plan create --name meuPlanoDeAppService --resource-group meuGrupoDeRecursos --sku FREE
```

### Crie um Web App

Crie uma aplicação web para hospedar a API FastAPI:

```bash
az webapp create --resource-group meuGrupoDeRecursos --plan meuPlanoDeAppService --name minhaAppFastAPI --runtime "PYTHON:3.9"
```

### Configure o Deploy via Git

Configure o deploy local via Git:

```bash
az webapp deployment source config-local-git --name minhaAppFastAPI --resource-group meuGrupoDeRecursos
```

O comando retornará uma URL para o repositório Git. Adicione essa URL como remoto:

```bash
git remote add azure <URL_DO_GIT_RETORNADA>
```

### Faça o Deploy da Aplicação

Envie o código para o Azure com o seguinte comando:

```bash
git push azure master
```

### Teste a Aplicação no Azure

Após o deploy, acesse a aplicação no seguinte endereço:

```bash
https://minhaAppFastAPI.azurewebsites.net
```

Você verá a seguinte resposta:

```json
{
  "message": "Hello, Azure!"
}
```

## Solução de Problemas

Caso encontre erros durante o deploy, verifique os logs com:

```bash
az webapp log tail --name minhaAppFastAPI --resource-group meuGrupoDeRecursos
```

Certifique-se de que a versão do Python corresponde à configurada no Azure Web App.

## Contribuição <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Travel%20and%20places/Rocket.png" alt="Rocket" width="25" height="25" />

Sinta-se à vontade para contribuir com melhorias neste projeto. Envie um pull request ou abra uma issue para discussão.
