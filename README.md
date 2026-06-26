🛒 Flipkart Product Chatbot: An LLMOps Journey
Welcome! This isn't just another standard "hello world" chatbot—it's a fully containerized, Kubernetes-orchestrated, LLM-powered shopping assistant built for scale.

💡 What exactly is this?
This project brings intelligent conversational search to Flipkart product data. Instead of forcing users to blindly click through endless categories and filters, they can simply talk to the bot to find exactly what they are looking for.

Under the hood, it is a robust LLMOps pipeline that pieces together modern AI tools with cloud-native infrastructure.

🧠 How it works (The Tech Stack)
I didn't just build a script and call it a day. I built a complete system:

The Brains: A Python/Flask backend supercharged by Large Language Models using the Groq API and Hugging Face.

The Memory: DataStax Astra DB handles the heavy lifting for vector-based information retrieval, giving the bot its context.

The Muscle: The entire application is containerized with Docker and orchestrated using Kubernetes (Minikube) on a Google Cloud (GCP) Ubuntu VM.

The Vitals: A complete observability stack using Prometheus and Grafana to monitor cluster health, track API latency, and visualize performance in real-time.

🚀 Why it matters
This repository bridges the gap between building an AI application and deploying one. It tracks the complete journey from raw code to a highly available, monitored microservice on the cloud. It's built to self-heal, scale, and give us beautiful operational graphs along the way.
