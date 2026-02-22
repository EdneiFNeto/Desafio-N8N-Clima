# 🌤️ Agente de Clima com n8n + Telegram

## 📌 Descrição

Este projeto implementa um **Agente de Clima** utilizando **n8n**
integrado ao **Telegram**.

O agente permite que usuários consultem a **temperatura atual e as
condições climáticas** de qualquer cidade do Brasil diretamente pelo
Telegram.

O fluxo utiliza: - Telegram para interação com o usuário - n8n para
orquestração - Agente de IA (OpenAI) para interpretação da mensagem -
Tavily Search para obtenção de informações atualizadas sobre o clima

O bot responde com: - 📍 Cidade\
- 🌡️ Temperatura atual\
- 🌤️ Condição do tempo

Exemplo de resposta:

📍 Cidade: Rio de Janeiro\
🌡️ Temperatura: 28°C\
🌧️ Condição: Pancadas de chuva à tarde

------------------------------------------------------------------------

## 🚀 Funcionalidades

-   Consulta de clima em **linguagem natural**
-   Suporte apenas para **cidades do Brasil**
-   Informações apenas do **dia atual**
-   Validação de erros:
    -   Cidade não informada
    -   País diferente do Brasil
    -   Perguntas fora de contexto
-   Respostas amigáveis com emojis
-   Integração completa via Telegram

------------------------------------------------------------------------

## 🏗️ Arquitetura

Telegram → Webhook → n8n → AI Agent (OpenAI) → Tavily Search → Resposta
Telegram

------------------------------------------------------------------------

## 📋 Pré-requisitos

-   n8n
-   Conta no Telegram
-   Conta no OpenAI (para o agente de IA)
-   Conta no Tavily
-   ngrok instalado

------------------------------------------------------------------------

## 🔑 Criando a chave da OpenAI

1.  Acesse: https://platform.openai.com/
2.  Crie uma conta ou faça login
3.  Vá em **API Keys**
4.  Clique em **Create new secret key**
5.  Copie a chave gerada

------------------------------------------------------------------------

## ⚙️ Configurando a OpenAI no n8n

1.  Abra o workflow
2.  No node **Chat Model / OpenAI**
3.  Crie uma nova credencial
4.  Cole sua **API Key**
5.  Salve

------------------------------------------------------------------------

## ▶️ Executando o projeto localmente

### 1️⃣ Iniciar o n8n

``` bash
n8n start
```

Acesse: http://localhost:5678

------------------------------------------------------------------------

### 2️⃣ Expor o ambiente com ngrok

O Telegram exige um endpoint **HTTPS público** para webhooks.

Instale: https://ngrok.com/

Execute:

``` bash
ngrok http 5678
```

Configure antes de iniciar o n8n:

Linux / Mac:

``` bash
export WEBHOOK_URL=https://sua-url-ngrok
n8n start
```

Windows (PowerShell):

``` powershell
$env:WEBHOOK_URL="https://sua-url-ngrok"
n8n start
```

------------------------------------------------------------------------

## 🤖 Configurando o Bot no Telegram

1.  Abra o Telegram
2.  Procure por **@BotFather**
3.  Execute:

```{=html}
    /start
    /newbot
```
    

4.  Defina:

-   Nome do bot
-   Username (terminando em `bot`)

5.  Copie o **Bot Token**

------------------------------------------------------------------------

## 🔄 Importando o Workflow

1.  Abra o n8n
2.  Clique em **Import**
3.  Selecione o arquivo JSON do workflow
4.  Configure as credenciais:
    -   OpenAI
    -   Telegram
    -   Tavily
5.  Ative o workflow

------------------------------------------------------------------------

## 🧪 Como usar

Envie no Telegram:

    Rio de Janeiro
    Qual o clima em Recife?
    Temperatura em São Paulo

------------------------------------------------------------------------

## ❌ Tratamento de erros

O agente retorna mensagens com 😢 quando:

-   A cidade não é informada
-   A cidade não é do Brasil
-   A pergunta não está relacionada a clima
-   Os dados não podem ser obtidos

------------------------------------------------------------------------

## 🛠️ Tecnologias utilizadas

-   n8n
-   OpenAI
-   Telegram Bot API
-   Tavily Search
-   ngrok

------------------------------------------------------------------------

## 📎 Observações

-   O **ngrok** é necessário para permitir que o Telegram acesse o n8n
    local via HTTPS.
-   Este projeto demonstra a integração de **Agentes de IA com automação
    e mensageria**.

------------------------------------------------------------------------

## 👨‍💻 Autor

Projeto desenvolvido como demonstração de automação com agentes de IA
utilizando n8n.
