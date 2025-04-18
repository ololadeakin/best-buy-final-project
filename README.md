# best-buy-final-project

## 📐 Updated Application Architecture

![Updated Architecture Diagram](./assets/bestbuy%20architecture.jpg)

This architecture features:
- A MongoDB instance running as a StatefulSet
- Multiple microservices:
  - **Store-Front** for customer-facing product browsing and order creation
  - **Store-Admin** for managing products (with AI assistance) and viewing orders
  - **Product-Service** for managing the available product catalog
  - **Order-Service** for handling incoming orders
  - **Makeline-Service** for managing the order processing queue
  - **AI-Service** for generating chat responses and image descriptions using GPT-4 and DALL·E-3 from Azure OpenAI
- Azure Event Bus for asynchronous communication between services
- All services are containerized, deployed to Kubernetes, and communicate over internal services

---

## 🧠 Application and Architecture Explanation

This microservices-based application handles e-commerce and restaurant orders, integrating AI capabilities:

- **Store-Front** allows customers to view products and create orders.
- **Store-Admin** enables staff to manage products using AI (for image descriptions or generating product details) and view order history.
- **Product-Service** stores and retrieves product information.
- **Order-Service** receives new orders and sends them to the Azure Event Bus.
- **Makeline-Service** listens to events, updates the queue, and manages preparation status.
- **AI-Service** connects to Azure OpenAI to:
  - Use **GPT-4** to generate chat responses and product descriptions.
  - Use **DALL·E-3** to describe or generate images for products.

All services connect to MongoDB for persistent data and use Kubernetes secrets for sensitive credentials.

---

## 🚀 Deployment Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/ololadeakin/best-buy-final-project.git
cd best-buy-final-project
```

### 2. Create Secrets
Encode your Azure Event Bus and OpenAI API keys:
```bash
echo -n '<event-bus-connection-string>' | base64

# For OpenAI API key
echo -n '<your-openai-api-key>' | base64
```

Create the secret file:
```yaml
# secrets.yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secrets
  namespace: default
type: Opaque
data:
  connectionString: <base64-event-bus-connection-string>
  openaiApiKey: <base64-openai-api-key>
```

Apply the secret:
```bash
kubectl apply -f secrets.yaml
```

### 3. Deploy Services
```bash
kubectl apply -f Deployment files/app-all-in-one.yaml
```

---

## 📁 Table of Microservice Repositories

| Service         | Repository Link                             |
|----------------|----------------------------------------------|
| Store-Front     | https://github.com/ololadeakin/store-front-final-project                              |
| Store-Admin     | https://github.com/ololadeakin/store-admin-final-project                               |
| Product-Service | https://github.com/ololadeakin/product-service-final-project                              |
| Order-Service   | https://github.com/ololadeakin/order-service-final-project   |
| Makeline-Service| https://github.com/ololadeakin/makeline-service-final-project|
| AI-Service      | https://github.com/ololadeakin/ai-service-final-project     |

---

## 🐳 Table of Docker Images

| Service         | Docker Image Link                                 |
|----------------|----------------------------------------------------|
| Store-Front     | https://hub.docker.com/r/lolaakin/store-front                                 |
| Store-Admin     | https://hub.docker.com/r/lolaakin/store-admin                                 |
| Product-Service | https://hub.docker.com/r/lolaakin/product-service                                 |
| Order-Service   | https://hub.docker.com/r/lolaakin/order-service   |
| Makeline-Service| https://hub.docker.com/r/lolaakin/makeline-service|
| AI-Service      | https://hub.docker.com/r/lolaakin/ai-service      |

---

## ⚠️ Known Issues or Limitations
- OpenAI API limits may apply for high-volume usage.


---

## 📂 Deployment Files
Ensure the following files are in the `Deployment files/` folder:

- `apps-all-in-one.yaml`
- `store-front-deployment.yaml`
- `store-admin-deployment.yaml`
- `product-service-deployment.yaml`
- `order-service-deployment.yaml`
- `makeline-service-deployment.yaml`
- `secrets.yaml`
- `config-map.yaml` 

