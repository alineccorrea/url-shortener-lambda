# Encurtador de URL

Este projeto é um encurtador de URL, desenvolvido com arquitetura serverless. Ele permite gerar URLs encurtadas e gerenciar seu tempo de expiração.

## Tecnologias

- **Java**: Desenvolvimento do backend.
- **AWS Lambda**: Computação serverless para o backend.
- **Amazon S3**: Armazenamento de metadados das URLs.
- **Amazon API Gateway**: Unificação das funções Lambda como APIs REST.
- **Biblioteca: jackson-core**: Para processamento de JSON.
- **Biblioteca: lombok**: Facilitador de desenvolvimento Java (produtividade e redução de código).

## Arquitetura

![Encurtador-URL-schema](https://github.com/user-attachments/assets/233f20df-6e05-48c1-a503-0824e3154821)


### Função CreateUrlLambda [POST]

- Valida os dados recebidos.
- Gera um código UUID curto.
- Salva o mapeamento dos dados no bucket S3 em formato JSON.

### Função RedirectUrlShortner [GET]

- Valida a chave UUID enviada pela requisição.
- Recupera os metadados para o código fornecido dentro dos dados salvos no S3.
- Verifica o tempo de expiração.
- **Redireciona para a url original ou retorna um erro caso a URL esteja expirada.**

## Endpoints

## URL: https://d28leejbzd.execute-api.us-east-1.amazonaws.com

### 1. Gerar URL Encurtada
- Método: `POST`
- Endpoint: `/create`
- Corpo da requisição: `{ "originalUrl": "https://URLexemplo.com", "expirationTime": "86400" }`
- Resposta: `{"code": "CódigoUUIDGerado"}`

### 2. Redirecionar URL 
- Método: `GET`
- Endpoint: `/{urlCode}`
- Resposta:
  - `status: 302` Redireciona para a URL original.
  - `status: 410` Indica que a URL expirou.

## Configuração do projeto

1. Configurar AWS.
2. Na IDE, criar dois projetos separados `CreateUrlLambda` e `RedirectUrlShortner`. Os projetos foram unificados aqui para facilitar a visualização das funções e acesso aos projetos.
3. Empacotar o projeto com Maven: `mvn clean package`.
4. Implemente funções Lambda e configure o API Gateway.
