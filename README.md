# Serverless-RAG-Chatbot-on-AWS-using-Terraform

This project is a **serverless Retrieval-Augmented Generation (RAG) chatbot** built on AWS that allows users to ask questions about **AWS Solutions Architect certification topics** using a **small PDF document** as the knowledge source.

![Alt Text](https://github.com/Naveen15github/Serverless-RAG-Chatbot-on-AWS/blob/9a4ee3ab27f236d2337555614017e63254476943/Gemini_Generated_Image_pfecp9pfecp9pfec.png)

The system uses **Amazon Titan Embeddings G1 – Text** to generate vector representations of the PDF content, stores them in **Amazon S3 Vectors**, and generates responses using **Meta LLaMA 3** via **AWS Bedrock**.
All infrastructure is managed with **Terraform**, following a secure, scalable, and fully serverless design.

![Alt Text](https://github.com/Naveen15github/Serverless-RAG-Chatbot-on-AWS/blob/9a4ee3ab27f236d2337555614017e63254476943/Screenshot%20(363).png)

---

## Project Objective

The main objective is to demonstrate a **complete RAG pipeline** using AWS-native services, without relying on third-party vector databases or hosting custom models.

Key goals:

* Convert a small AWS certification PDF into a searchable knowledge base
* Generate embeddings using **Titan Embeddings G1 – Text**
* Perform semantic retrieval using **Amazon S3 Vectors**
* Generate grounded answers using **Meta LLaMA 3**
* Maintain a secure, scalable, and serverless architecture

---

## Knowledge Source

* **Input Data**: Small PDF containing AWS Solutions Architect certification information
* **Document Storage**: Amazon S3
* **Embeddings Model**: Amazon Titan Embeddings G1 – Text
* **Vector Store**: Amazon S3 Vectors

The PDF acts as the **single source of truth**, ensuring all answers are strictly derived from certification content.

---

## High-Level Architecture Flow

1. User submits a question through the web chat interface
2. Request is routed via API Gateway
3. Lambda generates an embedding for the query using Titan Embeddings G1 – Text
4. Relevant vectors are retrieved from Amazon S3 Vectors
5. Retrieved context is sent to Meta LLaMA 3 via AWS Bedrock
6. A grounded response is returned to the user

---

## Services Used and Their Roles

### 1. Amazon S3 (Simple Storage Service)

**Role:**

* Stores the static frontend (HTML, CSS, JavaScript)
* Stores the AWS Solutions Architect PDF

**Why it is used:**

* Durable and cost-effective storage
* Native integration with Bedrock and S3 Vectors
* Ideal for static website hosting and document storage

![Alt Text](https://github.com/Naveen15github/Serverless-RAG-Chatbot-on-AWS/blob/9a4ee3ab27f236d2337555614017e63254476943/Screenshot%20(368).png)

---

### 2. Amazon Titan Embeddings G1 – Text

**Role:**

* Converts PDF text and user queries into vector embeddings

**Why it is used:**

* Fully managed embedding model
* Optimized for semantic search
* Ensures embeddings are consistent for RAG retrieval

**In this project:**

* Document chunks are embedded into vectors
* User queries are embedded in real time for similarity search

---

### 3. Amazon S3 Vectors

**Role:**

* Stores vector embeddings
* Performs semantic retrieval based on similarity

**Why it is used:**

* Fully managed AWS-native vector storage
* Eliminates the need for external vector databases
* Scales automatically

**In this project:**

* Embeddings from Titan are stored as vectors
* Relevant vectors are retrieved during query time to provide context to the model

---

### 4. Amazon CloudFront

![Alt Text](https://github.com/Naveen15github/Serverless-RAG-Chatbot-on-AWS/blob/9a4ee3ab27f236d2337555614017e63254476943/Screenshot%20(365).png)


**Role:**

* Distributes the frontend globally with low latency

**Why it is used:**

* Improves performance
* Adds security layer in front of S3
* Reduces direct bucket exposure

---

### 5. Amazon API Gateway (HTTP API)

![Alt Text](https://github.com/Naveen15github/Serverless-RAG-Chatbot-on-AWS/blob/9a4ee3ab27f236d2337555614017e63254476943/Screenshot%20(366).png)

**Role:**

* Exposes a `/query` endpoint for chat requests

**Why it is used:**

* Managed API service
* Handles CORS and routing
* Securely integrates with Lambda

---

### 6. AWS Lambda

![Alt Text](https://github.com/Naveen15github/Serverless-RAG-Chatbot-on-AWS/blob/9a4ee3ab27f236d2337555614017e63254476943/Screenshot%20(367).png)


**Role:**

* Orchestrates the RAG workflow

**Why it is used:**

* Serverless execution
* Automatic scaling
* Cost-efficient

**Responsibilities:**

* Receives user queries
* Generates embeddings using Titan Embeddings G1 – Text
* Retrieves relevant vectors from S3 Vectors
* Constructs prompts for LLaMA 3
* Returns responses to the frontend

---

### 7. AWS Bedrock

![Alt Text](https://github.com/Naveen15github/Serverless-RAG-Chatbot-on-AWS/blob/9a4ee3ab27f236d2337555614017e63254476943/Screenshot%20(364).png)


**Role:**

* Provides access to foundation models
* Runs the generation part of RAG

**Why it is used:**

* Secure enterprise-grade LLM access
* Native integration with vector stores
* No infrastructure management required

---

### 8. Meta LLaMA 3 (`meta.llama3-2-11b-instruct-v1`)

**Role:**

* Generates natural language responses based on retrieved context

**Why it is used:**

* Strong instruction-following
* Efficient for focused knowledge bases
* Suitable for private AI assistants

---

### 9. AWS IAM (Identity and Access Management)

**Role:**

* Manages service-to-service permissions

**Why it is used:**

* Enforces least-privilege access
* Secures Lambda, S3, Bedrock, and API Gateway interactions

---

### 10. Terraform (Infrastructure as Code)

![Alt Text](https://github.com/Naveen15github/Serverless-RAG-Chatbot-on-AWS/blob/9a4ee3ab27f236d2337555614017e63254476943/Screenshot%20(355).png)
![Alt Text](https://github.com/Naveen15github/Serverless-RAG-Chatbot-on-AWS/blob/9a4ee3ab27f236d2337555614017e63254476943/Screenshot%20(356).png)


**Role:**

* Automates the deployment of all resources

**Why it is used:**

* Repeatable and version-controlled deployments
* Simplifies infrastructure management
* Enables clean teardown

---

## RAG Workflow (Titan + S3 Vectors)

1. User submits a certification-related question
2. Query is embedded using Titan Embeddings G1 – Text
3. Similar vectors are retrieved from S3 Vectors
4. Retrieved content is injected into the prompt
5. Meta LLaMA 3 generates a grounded response
6. Answer is returned to the user

This ensures **high relevance and minimal hallucination**, using **AWS-native embeddings and vector storage**.

---

## Project Structure

```
.
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── lambda/
│   ├── index.py
│   └── requirements.txt
├── frontend/
│   ├── index.html
│   ├── styles.css
│   └── app.js
├── deploy.sh
└── README.md
```

---

## Security Considerations

* S3 Vectors are not publicly accessible
* IAM roles follow least-privilege principle
* Serverless architecture reduces attack surface
* Optional API authentication can be added

---

## Author

**Naveen G**
Cloud | Serverless | AI Engineering

---

## Final Note

This project demonstrates a **modern AWS-native RAG implementation** using **Titan Embeddings G1 – Text and S3 Vectors**, showing how even a **small certification PDF** can become a **private, AI assistant** without external databases or custom infrastructure.

