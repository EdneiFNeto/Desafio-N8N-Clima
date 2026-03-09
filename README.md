# 🌤️ Agente de Clima com n8n + Telegram

## 📌 Descrição

Este projeto implementa um **Agente de Clima** utilizando **n8n**
integrado ao **Telegram**.

O agente permite que usuários consultem a **temperatura atual e as
condições climáticas** de qualquer cidade do Brasil diretamente pelo
Telegram.

### 🔄 Fluxo da aplicação

-   Telegram → interação com o usuário
-   n8n → orquestração do fluxo
-   OpenAI → interpretação da mensagem
-   OpenWeather → dados climáticos atualizados

📌 Exemplo de resposta:

> 🌧️ A temperatura em Minas Gerais é de 18°C com chuva leve.

------------------------------------------------------------------------

# 📋 Pré-requisitos

Antes de começar, você precisa ter:

-   n8n instalado
-   Conta no Telegram
-   Conta na OpenAI
-   Conta na OpenWeather
-   ngrok instalado

------------------------------------------------------------------------

# 📥 Importando o Workflow no n8n

Antes de configurar as credenciais, é necessário **importar o workflow
do projeto para dentro do n8n**.

O n8n permite importar workflows de duas formas: **via arquivo** ou
**via URL**.

## 🔹 Importar Workflow via Arquivo

1.  Abra o **n8n** no navegador:
    http://localhost:5678

2.  No menu lateral clique em **Workflows**
3.  Clique em **Import**
4.  Selecione **From File**
5.  Escolha o arquivo `.json` do workflow
6.  Clique em **Import**

------------------------------------------------------------------------

## 🔹 Importar Workflow via URL

1.  Abra o **n8n**
2.  Clique em **Workflows**
3.  Clique em **Import**
4.  Selecione **From URL**
5.  Cole a URL do arquivo `.json`
6.  Clique em **Import**

------------------------------------------------------------------------

# 🔑 Criando as API Keys

## 🔹 OpenAI

1.  Acesse: https://platform.openai.com/
2.  Faça login
3.  Vá em **API Keys**
4.  Clique em **Create new secret key**
5.  Copie a chave gerada

------------------------------------------------------------------------

## 🔹 OpenWeather

1.  Acesse: https://openweathermap.org/
2.  Faça login
3.  Vá em **API Keys**
4.  Clique em **Create Key**
5.  Copie sua chave

⚠️ A ativação pode levar alguns minutos.

------------------------------------------------------------------------

# 🤖 Criando o Bot no Telegram

1.  Abra o Telegram
2.  Procure por **@BotFather**
3.  Execute:

```{=html}
/start
/newbot
```
    
4.  Defina:

-   Nome do bot
-   Username (deve terminar com `bot`)

5.  Copie o **Bot Token**

------------------------------------------------------------------------

# 🔐 Configurando Credenciais no n8n

O **n8n** utiliza o **Credential Manager** para armazenar tokens e
chaves de API.

## 🔹 Credencial do Telegram

1.  No **n8n**, vá em **Credentials**
2.  Clique em **Create Credential**
3.  Escolha **Telegram API**
4.  No campo **Access Token**, cole o **Bot Token** obtido com o
    **BotFather**
5.  Clique em **Save**

------------------------------------------------------------------------

## 🔹 Credencial da OpenAI

1.  Abra o workflow
2.  Clique no node **OpenAI**
3.  Em **Credentials**, clique em **Create New**
4.  Cole sua **API Key**
5.  Clique em **Save**

------------------------------------------------------------------------

# ⚙️ Configurando o Node HTTP Request (OpenWeather)

**Método:** GET

URL:

    https://api.openweathermap.org/data/2.5/weather

## Query Parameters

  Nome    Valor
  ------- -------------------------------
  q       Cidade,UF,BR
  appid   {{\$env.OPENWEATHER_API_KEY}}
  units   metric
  lang    pt_br

------------------------------------------------------------------------

# ▶️ Executando o Projeto Localmente

## Iniciar o n8n

``` bash
n8n start
```

Acesse:

    http://localhost:5678

------------------------------------------------------------------------

## Expor com ngrok

``` bash
ngrok http 5678
```

Copie a URL HTTPS gerada.

------------------------------------------------------------------------

# 🔑 Variáveis e Credenciais

Para que o workflow funcione corretamente logo após a importação,
algumas informações precisam estar configuradas.

## Credenciais no n8n

| Serviço   | Informação necessária |
|-----------|-----------------------|
| Telegram  | Bot Token             |
|  OpenAI   | API Key               |

## Variáveis de ambiente

| Variável             | Descrição              |
|--------------------- | -----------------------|
|  OPENWEATHER_API_KEY   | API Key do OpenWeather |
| WEBHOOK_URL          | URL pública do ngrok. |

Exemplo:

``` bash
export OPENWEATHER_API_KEY=sua_api_key
export WEBHOOK_URL=https://sua-url-ngrok
```

------------------------------------------------------------------------

# 🧪 Como Usar

No Telegram envie:

    Rio de Janeiro, RJ

O bot responderá com a temperatura atual e descrição do clima.

------------------------------------------------------------------------

# 🛠️ Tecnologias Utilizadas

-   n8n
-   OpenAI
-   Telegram Bot API
-   OpenWeather
-   ngrok
