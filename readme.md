

`README.md`.

-----

# Guia de Uso da API OpenWeatherMap

Este repositório contém a documentação e exemplos de uso para a API de dados meteorológicos da OpenWeatherMap, como parte de um desafio técnico para a vaga de Analista de Suporte Júnior.

O objetivo é fornecer um guia claro e prático para que qualquer desenvolvedor possa começar a usar esta API.

## Índice

1.  [O que é a OpenWeatherMap API?](#o-que-é-a-openweathermap-api)
2.  [Pré-requisitos](#pré-requisitos)
3.  [Autenticação](#autenticação)
4.  [Endpoints Principais](#endpoints-principais)
      * [Obter Tempo Atual (`/weather`)](#1-obter-tempo-atual-weather)
      * [Obter Previsão para 5 Dias (`/forecast`)](#2-obter-previsão-para-5-dias-forecast)
5.  [Exemplo de Uso com Postman](#exemplo-de-uso-com-postman)
6.  [Tratamento de Erros Comuns](#tratamento-de-erros-comuns)
7.  [Limites de Uso](#limites-de-uso)

## O que é a OpenWeatherMap API?

A OpenWeatherMap é uma API RESTful que fornece acesso a dados meteorológicos globais. Com ela, é possível obter informações como temperatura atual, umidade, velocidade do vento, previsões de curto e longo prazo, e dados históricos para qualquer localização no mundo.

É amplamente utilizada em aplicações web e mobile, desde simples widgets de tempo até complexos sistemas de logística e agricultura.

## Pré-requisitos

Antes de começar a usar a API, você precisará de uma **Chave de API (API Key)**.

1.  **Crie uma conta gratuita** no site [openweathermap.org](https://openweathermap.org/).
2.  Após o login, navegue até a aba **"My API Keys"** no menu do seu perfil.
3.  Sua chave padrão ("Default") já estará gerada. Copie esta chave.

> **Atenção:** Após a criação, a chave de API pode levar de alguns minutos a até 2 horas para ser ativada pelos servidores. Se você receber um erro de "chave inválida" (`401 Unauthorized`), aguarde um pouco antes de tentar novamente.

## Autenticação

Todas as chamadas para a API precisam ser autenticadas. A autenticação é feita enviando sua Chave de API como um parâmetro de consulta (`query parameter`) em todas as requisições.

O nome do parâmetro é `appid`.

**Exemplo:**
`https://api.openweathermap.org/data/2.5/weather?q=london&appid=SUA_CHAVE_AQUI`

## Endpoints Principais

A URL base para os endpoints da versão 2.5 é: `https://api.openweathermap.org/data/2.5/`

A seguir, estão detalhados os endpoints mais comuns, disponíveis no plano gratuito.

### 1\. Obter Tempo Atual (`/weather`)

Este endpoint retorna os dados meteorológicos atuais para uma localização específica.

  * **Método:** `GET`
  * **Endpoint:** `/weather`

**Parâmetros Principais:**

| Parâmetro | Descrição | Exemplo |
| :--- | :--- | :--- |
| `q` | Nome da cidade e, opcionalmente, código do país. | `passos,br` |
| `lat`, `lon` | Coordenadas geográficas (latitude e longitude). | `lat=-20.71`, `lon=-46.60` |
| `appid` | **(Obrigatório)** Sua Chave de API. | `8a4f6f...` |
| `units` | Unidade de medida. `metric` para Celsius, `imperial` para Fahrenheit. | `metric` |
| `lang` | Idioma da resposta. | `pt_br` |

**Exemplo de Chamada Completa:**

`https://api.openweathermap.org/data/2.5/weather?q=passos,br&units=metric&lang=pt_br&appid=SUA_CHAVE_AQUI`

**Exemplo de Resposta (JSON):**

```json
{
  "coord": {
    "lon": -46.6094,
    "lat": -20.7194
  },
  "weather": [
    {
      "id": 800,
      "main": "Clear",
      "description": "céu limpo",
      "icon": "01d"
    }
  ],
  "main": {
    "temp": 25.4,
    "feels_like": 24.9,
    "temp_min": 24.8,
    "temp_max": 26.1,
    "pressure": 1012,
    "humidity": 45
  },
  "wind": {
    "speed": 3.1,
    "deg": 110
  },
  "name": "Passos"
}
```

### 2\. Obter Previsão para 5 Dias (`/forecast`)

Este endpoint retorna a previsão do tempo para os próximos 5 dias, com dados segmentados a cada 3 horas.

  * **Método:** `GET`
  * **Endpoint:** `/forecast`

**Parâmetros:** Utiliza os mesmos parâmetros de localização (`q`, `lat`, `lon`), autenticação e formatação do endpoint `/weather`.

**Exemplo de Chamada Completa:**

`https://api.openweathermap.org/data/2.5/forecast?q=passos,br&units=metric&lang=pt_br&appid=SUA_CHAVE_AQUI`

**Exemplo de Resposta (JSON - estrutura principal):**

```json
{
  "cod": "200",
  "message": 0,
  "cnt": 40,
  "list": [
    {
      "dt": 1668186000,
      "main": {
        "temp": 22.4,
        "feels_like": 22.3,
        "humidity": 68
      },
      "weather": [
        {
          "main": "Clouds",
          "description": "nuvens dispersas",
          "icon": "03d"
        }
      ],
      "dt_txt": "2025-11-11 18:00:00"
    },
    {
      "dt": 1668196800,
      "main": {
        "temp": 21.1,
        // ... mais dados ...
      },
      "weather": [
        // ... mais dados ...
      ],
      "dt_txt": "2025-11-11 21:00:00"
    }
    // ... mais 38 entradas de previsão
  ],
  "city": {
    "id": 3455323,
    "name": "Passos",
    "coord": {
      "lat": -20.7194,
      "lon": -46.6094
    },
    "country": "BR"
  }
}
```

## Exemplo de Uso com Postman

1.  Abra o Postman e crie uma nova requisição (`New Request`).
2.  Defina o método como **GET**.
3.  Cole a URL do endpoint desejado, por exemplo: `https://api.openweathermap.org/data/2.5/weather`.
4.  Vá para a aba **"Params"** e adicione os parâmetros como chaves e valores. Não use chaves `{}` ou aspas.

| KEY | VALUE |
| :--- | :--- |
| `q` | `passos,br` |
| `appid` | `SUA_CHAVE_AQUI` |
| `units` | `metric` |
| `lang` | `pt_br` |

5.  Clique em **"Send"** para executar a chamada e ver a resposta JSON no painel inferior.

## Tratamento de Erros Comuns

| Código | Mensagem | Causa Provável |
| :--- | :--- | :--- |
| `200 OK` | (Sucesso) | A requisição foi bem-sucedida. |
| `401 Unauthorized` | `Invalid API key` | A chave de API está incorreta, não foi enviada ou ainda não foi ativada. |
| `404 Not Found` | `city not found` | A cidade ou localização especificada não foi encontrada. |

## Limites de Uso

O plano gratuito (`Free`) da OpenWeatherMap possui as seguintes limitações:

  * **60 chamadas por minuto.**
  * **1.000.000 de chamadas por mês.**

Para aplicações com alto volume de tráfego, é necessário assinar um dos planos pagos.