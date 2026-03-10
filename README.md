
# 🌤️ Agente de Clima com n8n + Telegram

## 📌 Descrição

Este projeto demonstra como criar um **chatbot no Telegram utilizando n8n** para consultar informações de clima através de uma API de previsão do tempo.

O bot recebe uma **cidade enviada pelo usuário no Telegram** e responde com as **informações meteorológicas atuais**.

---

# 📋 Pré-requisitos

Antes de iniciar, você precisará ter instalado:

* Node.js
* n8n
* Conta no **Telegram**
* Conta em uma **API de clima**
* **ngrok** (para expor o webhook do n8n)

---

# 📂 Estrutura do Projeto

```

telegram-weather-chatbot
│
├── workflow-telegram-chatbot.json
└── README.md

```

Arquivo principal do workflow:

```

workflow-telegram-chatbot.json

```

Este arquivo contém todo o **workflow configurado para o n8n**.

---

# 📥 Importando o Workflow no n8n

Antes de configurar o bot, é necessário **baixar o workflow do projeto**.

Baixe o arquivo:

```

workflow-telegram-chatbot.json

````

Após baixar o arquivo, siga os passos abaixo para importá-lo no **n8n**.

---

## 🔹 Abrindo o n8n

Primeiro inicie o **n8n** com o comando:

```bash
n8n start
````

O n8n será iniciado localmente na porta padrão:

```
http://localhost:5678
```

Abra esse endereço no navegador para acessar a interface do n8n.

---

## 🌐 Expondo o n8n para o Telegram usando ngrok

O Telegram precisa acessar o **webhook do n8n pela internet**.
Como o n8n está rodando localmente, precisamos utilizar o **ngrok** para criar um túnel público.

### Baixar o ngrok

Acesse:

[https://ngrok.com/download](https://ngrok.com/download)

Baixe a versão correspondente ao seu sistema operacional e instale na sua máquina.

---

### Autenticar o ngrok

Após criar uma conta no ngrok, copie seu **authtoken** no painel do site e execute:

```bash
ngrok config add-authtoken SEU_TOKEN
```

---

### Expor a porta do n8n

Como o n8n está rodando na porta **5678**, execute:

```bash
ngrok http 5678
```

O ngrok irá gerar uma URL pública semelhante a:

```
https://abcd-1234.ngrok-free.app
```

---

### Usar a URL do ngrok no n8n

No **Telegram Trigger** dentro do n8n, utilize a URL gerada pelo ngrok como base para o webhook.

Exemplo:

```
https://abcd-1234.ngrok-free.app
```

Isso permitirá que o **Telegram envie mensagens para o seu workflow local**.

---

## 🔹 Importar Workflow via Arquivo

1. Abra o **n8n**
2. No menu lateral clique em **Workflows**
3. Clique em **Import**
4. Selecione **From File**
5. Escolha o arquivo:

```
workflow-telegram-chatbot.json
```

6. Clique em **Import**

O workflow será carregado no editor.

---

# 🤖 Criando o Bot no Telegram

Neste passo você vai precisar criar um bot no Telegram para conseguir utilizá-lo no chat.

1. Abra o Telegram
2. Procure por:

```
@BotFather
```

3. Execute o comando:

```
/start
```

4. Depois execute:

```
/newbot
```

5. Escolha:

* Nome do bot
* Username do bot

Exemplo:

```
Weather Bot
weather_example_bot
```

Após criar o bot, o **BotFather fornecerá um Token** semelhante a:

```
123456789:AAExampleTokenExample
```

Guarde esse token.

---

# 🔑 Configurando Credenciais do Telegram no n8n

No workflow importado:

1. Abra o node **Telegram Trigger** e, em **Credential to connect with**, selecione a opção **Create a new Token**

<img width="937" height="787" alt="image" src="https://github.com/user-attachments/assets/5c1be133-6297-44d3-b74c-d46c2231d4d5" />

2. Em **Telegram account**, no campo **Access Token**, adicione seu token gerado no BotFather.

<img width="762" height="691" alt="image" src="https://github.com/user-attachments/assets/3ec9fdf8-5672-42c7-a6b5-777ade74e67c" />

3. Clique em salvar e pronto, o Telegram estará pronto para ser utilizado.

<img width="747" height="675" alt="image" src="https://github.com/user-attachments/assets/29cd058c-e949-4b65-abc5-a156c05e0e42" />

⚠️ Agora nos outros nodes (**Send a text message - empty message**, **Send a text message - Error response message** e **Send a text message - clima**) basta selecionar **Telegram account** na opção **Credential to connect with**.

<img width="836" height="280" alt="image" src="https://github.com/user-attachments/assets/c2b90f30-c03b-48e6-b769-6a8e667ef0c1" />

---

# 🌤️ Criando Conta na API de Clima OpenWeather

Neste passo você vai precisar ter uma API Key para conseguir realizar chamadas na API de clima da OpenWeather.

1. Acesse: [https://openweathermap.org/](https://openweathermap.org/)
2. Faça login
3. Vá em **API Keys**
4. Clique em **Create Key**
5. Copie sua WEATHER_KEY_API

⚠️ A ativação pode levar alguns minutos.

Exemplo:

```
abc123exampleapikey
```

---

# ⚙️ Configurando o Node WeatherAPI no n8n

No workflow existe um node responsável por consultar o clima.

Configure da seguinte forma.

**Node:** `HTTP Request`

### Método

```
GET
```

### URL

```
https://api.openweathermap.org/data/2.5/weather
```

### Query Parameters

Adicione os parâmetros:

| Nome  | Valor                        |
| ----- | ---------------------------- |
| appid | WEATHER_KEY_API              |
| q     | {{$json["message"]["text"]}} |
| units | metric                       |
| lang  | pt                           |

Explicação:

* **appid** → sua WEATHER_KEY_API cadastrada na seção Criando Conta na API de Clima OpenWeather.
* **q** → cidade enviada pelo usuário no Telegram
* **units=metric** → temperatura em Celsius
* **lang=pt** → resposta em português

Exemplo:

Dentro do node WeatherAPI em **Query Parameters**, basta adicionar o query parameter **appid** com a **SUA_WEATHER_KEY_API**, explicada na seção Criando Conta na API de Clima OpenWeather.

<img width="1278" height="803" alt="image" src="https://github.com/user-attachments/assets/5683cf00-0fca-4662-9a36-93f397aae586" />

---

# 🤖 Configurando API Key da OpenAI no n8n

Aqui está um **passo a passo simples para obter sua API Key da OpenAI na OpenAI Platform** 🔑 e configurá-la no n8n.

---

## Criar ou acessar sua conta

1. Acesse: [https://platform.openai.com](https://platform.openai.com)
2. Clique em **Sign up** para criar conta ou **Log in** se já tiver uma.
3. Você pode entrar usando:

* Google
* Microsoft
* Apple
* Email e senha

---

## Acessar a página de API Keys

1. Após entrar na plataforma, abra diretamente:
   [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Essa página mostra **todas as chaves já criadas**.

---

## Criar uma nova API Key

1. Clique no botão **Create new secret key**.
2. Dê um **nome para a chave** (exemplo: `meu-app-teste`).
3. Clique em **Create**.

---

## Copiar e salvar a chave

* A chave será exibida **apenas uma vez**.
* Copie e guarde em local seguro.

Exemplo de formato:

```text
sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

⚠️ **Importante**

* Nunca publique essa chave no GitHub.
* Use **variáveis de ambiente** no seu projeto.

---

## Adicionar API Key no n8n

1. Dentro do node **AI Agent**, no **Chat Model (OpenAI Chat Model)**, na opção **Credential to connect with**, selecione **Create a new Credential**.

<img width="1130" height="669" alt="image" src="https://github.com/user-attachments/assets/18f42247-8f11-416d-97c1-b6f5dfc63cab" />

2. Em **OpenAI account**, digite sua API Key depois clique em salvar. 

<img width="820" height="700" alt="image" src="https://github.com/user-attachments/assets/fa36d2f6-fbe1-4e95-b490-4ab9bd2b2735" />

---


# 💬 Configurando a Resposta do Bot

No node **Telegram Send Message**, configure o campo **Text** com algo semelhante a:

```
🌤️ Clima em {{$json["name"]}}

Temperatura: {{$json["main"]["temp"]}} °C
Sensação térmica: {{$json["main"]["feels_like"]}} °C
Umidade: {{$json["main"]["humidity"]}}%
Condição: {{$json["weather"][0]["description"]}}
```

---

# ▶️ Ativando o Workflow

Depois de configurar tudo:

1. Clique no botão **Activate** no topo do n8n
2. Abra o Telegram
3. Envie uma cidade para o bot

Exemplo:

```
Rio de Janeiro
```

Resposta esperada:

```
🌤️ Clima em Rio de Janeiro

Temperatura: 29°C
Sensação térmica: 31°C
Umidade: 70%
Condição: céu parcialmente nublado
```
