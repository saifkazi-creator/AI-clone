Make an AI Clone of Yourself
This project provides a comprehensive guide to creating a personalized AI chatbot using your own WhatsApp chat data. By fine-tuning a large language model (LLM) like Llama 3 on your conversation history, you can build a model that communicates in your unique style, including a mix of languages like Hinglish (Hindi + English).

The project addresses privacy concerns by ensuring that your data is not sent to third-party APIs. The entire fine-tuning process is performed for free using a Google Colab instance, and the final model can be run locally on your computer with Ollama.

Motivation
Have you ever wondered if you could have an AI that talks just like you? This project was inspired by an AI enthusiast who used Retrieval Augmented Generation (RAG) and the GPT-3.5 Turbo API to create a personal AI clone. However, this method has a major drawback: sending personal chats to OpenAI for processing can pose significant privacy risks.

This guide provides an alternative approach. Instead of using a third-party API, we'll fine-tune a pre-existing model, ensuring your data remains completely private and secure. Fine-tuning a large model, such as one with 7 billion parameters, typically requires powerful hardware with a minimum of 32 GB of GPU RAM, which is costly and not available in free-tier GPU compute services. However, this project demonstrates how to work around these limitations using techniques like 4-bit quantization and free-tier GPU services like Google Colab.

Getting Started
1. Data Collection and Preparation
The first step is to collect your WhatsApp chat history and prepare it for fine-tuning.

Export Your Chats: Open WhatsApp, go to the chat you want to export, and select More > Export Chat and choose "Without media". Repeat this process for all individual chats you want to use. The more data you have, the better the model will perform.

Filter Your Data: The exported chat files contain irrelevant information like timestamps, omitted media messages, and deleted messages. This project uses regex to automatically remove these elements and convert the chat history into a Prompt: Response format. You can also filter out specific "filler words" (e.g., "Ok," "Yup," "Hmm") to prevent the model from learning to respond with these words exclusively.

2. Fine-Tuning the Model
The fine-tuning process is done using a Google Colab notebook, making it accessible and free. We'll use the Unsloth library, which is highly optimized for fine-tuning and requires less VRAM.

Open the Colab Notebook: The notebook contains all the code needed for data preparation and model training.

Upload Your Chat Files: Upload the exported chat .zip files to the Colab runtime environment.

Run the Notebook: The notebook will guide you through the process of data preparation, filtering, and model training. It uses Llama-3 8B Instruct as the base model, fine-tuning it with a technique called 4-bit Quantization to reduce memory usage. The notebook will train the model and save the fine-tuned version in a GGUF format, which is compatible with local LLM runners like Ollama.

3. Running the Model Locally with Ollama
Once the fine-tuning is complete, you can download the model and run it on your personal computer.

Install Ollama: Download and install Ollama from the official website.

Download the Model: Download the unsloth.Q8_0.gguf file and the Modelfile from your Google Colab session. You can do this directly from the Colab file browser or by copying the files to your Google Drive for easier downloading.

Load the Model into Ollama: Open a terminal in the same directory where you downloaded the model files. Edit the Modelfile to ensure the first line is FROM ./unsloth.Q8_0.gguf, then save it. Run the command: ollama create my_model -f Modelfile. Your personalized model is now loaded into Ollama and can be run with ollama run my_model.

Automating WhatsApp with the AI Model
This project includes a Python script that uses the WPP_Whatsapp library to connect your fine-tuned model to WhatsApp, allowing it to respond to incoming messages automatically.

Clone the Repository:
git clone https://github.com/Eviltr0N/Make-AI-Clone-of-Yourself.git

Install Dependencies:
pip install -r requirements.txt

Run the ai_to_whatsapp.py script:
python3 ai_to_whatsapp.py

Connect Your WhatsApp: A browser window will open, prompting you to scan a QR code with your WhatsApp app to link your account. Once connected, the script will ask for the phone number of the person you want the AI to respond to (including the country code, without the + symbol).

Customization
You can adjust the behavior of your AI by editing the ai_to_whatsapp.py file.

Temperature: Controls the creativity and unpredictability of the model's responses. A higher value (e.g., 0.9) makes responses more creative, while a lower value (e.g., 0.3) makes them more focused and predictable.

Top_k: Determines the number of top candidate tokens to consider for the next word. A higher value (e.g., 95) allows for more diverse responses.

What's Next
Multimodal Capabilities: Add support for the model to understand and respond to images, not just text.

Agent Pipeline: Implement an agent-based system where a secondary model (e.g., Llama 3.1) can review and refine the responses of the fine-tuned model to ensure they are suitable and accurate.
