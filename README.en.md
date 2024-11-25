# URL Shortener

This project is a URL shortener developed with a serverless architecture. It allows you to generate shortened URLs and manage their expiration time.

## Technologies

- **Java**: Backend development.  
- **AWS Lambda**: Serverless computing for the backend.  
- **Amazon S3**: Metadata bucket storage for URLs.  
- **Amazon API Gateway**: Integration of Lambda functions as REST APIs.  
- **Library: jackson-core**: For JSON processing.  
- **Library: lombok**: Java development facilitator (productivity and code reduction).

## Architecture

![URL Shortener Schema](https://github.com/user-attachments/assets/233f20df-6e05-48c1-a503-0824e3154821)

### Class: CreateUrlLambda [POST]

- Validates the received data.  
- Generates a short UUID code.  
- Saves the data mapping in the S3 bucket in JSON format.

### Class: RedirectUrlShortner [GET]

- Validates the UUID key provided in the request.  
- Retrieves the metadata for the provided code from the data stored in S3.  
- Checks the expiration time.  
- **Redirects to the original URL or returns an error if the URL has expired.**

## Endpoints

### Base URL: https://d28leejbzd.execute-api.us-east-1.amazonaws.com  

### 1. Generate Shortened URL
- Method: `POST`  
- Endpoint: `/create`  
- Request body: `{ "originalUrl": "https://exampleURL.com", "expirationTime": "86400" }`  
- Response: `{"code": "GeneratedUUIDCode"}`  

### 2. Redirect URL
- Method: `GET`  
- Endpoint: `/{urlCode}`  
- Response:  
  - `status: 302` Redirects to the original URL.  
  - `status: 410` Indicates that the URL has expired.  

## Project Setup

1. Configure AWS.  
2. Package the project with Maven: `mvn clean package`.  
3. Deploy Lambda functions and configure the API Gateway.