# AI-Engineer-Level1-RAG-System-Rejaul_Islam_Shanto
This project builds a bilingual (English/Bengali) RAG system using a chapter from Tagore’s Oporichita. The text is chunked by sentence for processing.

#Setup Guide
This project completely doing in Google Colab. Firstly create a colab notebook. Upload there HSC26_chunks.txt file in drive and copy there file path form sidebar of colab folder. 
After that paste there file path to there coding line accordingly. Then simply run there cells and see there outputs.

#Tools, Library and Packages
Google Colab: Cloud-based Python environment with Google Drive integration.
Flask: Lightweight web framework for API endpoints (/chat, /health, /batch_chat, /stats).
ngrok: Exposes local server via public URL.
LangChain: Provides HuggingFaceEmbeddings for text embeddings (use langchain-huggingface for updates).
Hugging Face Hub: Downloads pre-trained models/tokenizers.
Requests: Handles HTTP requests for API testing.
Pyngrok: Manages ngrok tunnels.
Jupyter Widgets: Displays download progress.
Werkzeug: Flask's WSGI utility for server operations.

#Sample Queries and Outputs

<img width="1919" height="1079" alt="Screenshot 2025-07-26 135842" src="https://github.com/user-attachments/assets/da4e597c-ec8c-4b3d-9d00-ebf8220bb908" />
<img width="1919" height="1079" alt="Screenshot 2025-07-26 135815" src="https://github.com/user-attachments/assets/a2ca82fc-9a10-400d-abab-4982663bccfc" />
<img width="1919" height="1079" alt="Screenshot 2025-07-26 123540" src="https://github.com/user-attachments/assets/4173af2f-1325-412b-8acc-46038d534e9c" />

#Simple Conversational API Documentation

Base URL
The base URL is dynamically generated via ngrok (e.g., https://<ngrok-id>.ngrok-free.app). Use get_current_api_url() to retrieve the active URL.
1. Health Check

Endpoint: GET /health
Description: Checks the API's status and availability.
Request:
Method: GET
Headers: None
Body: None

Response:
Status: 200 OK (success), 500 (error)
Body (JSON):{
  "status": "healthy",
  "service": "RAG API"
}

Example:curl https://<ngrok-id>.ngrok-free.app/health

2. Chat
Endpoint: POST /chat
Description: Submits a single question to the RAG system and returns an answer with confidence and optional sources.
Request:
Method: POST
Headers: Content-Type: application/json
Body (JSON):{
  "message": "Your question here",
  "include_sources": true
}

Response:
Status: 200 OK (success), 400 (bad request), 500 (error)
Body (JSON):{
  "message": "Original question",
  "response": "Answer from RAG system",
  "confidence": 0.95,
  "sources": ["Source text 1...", "Source text 2..."]
}
Example:curl -X POST https://<ngrok-id>.ngrok-free.app/chat \
     -H "Content-Type: application/json" \
     -d '{"message": "অনুপমের ভাষায় সুপুরুষ কাকে বলা হয়েছে?", "include_sources": true}'

3. Batch Chat
Endpoint: POST /batch_chat
Description: Submits multiple questions and returns answers with confidence and sources.
Request:
Method: POST
Headers: Content-Type: application/json
Body (JSON):{
  "messages": ["Question 1", "Question 2", ...],
  "include_sources": true
}

Response:
Status: 200 OK (success), 400 (bad request), 500 (error)
Body (JSON):[
  {
    "message": "Question 1",
    "response": "Answer 1",
    "confidence": 0.92,
    "sources": ["Source 1...", "Source 2..."]
  },
  ...
]
Example:curl -X POST https://<ngrok-id>.ngrok-free.app/batch_chat \
     -H "Content-Type: application/json" \
     -d '{"messages": ["Question 1", "Question 2"], "include_sources": true}'

4. Statistics
Endpoint: GET /stats
Description: Retrieves API statistics, including model details and supported languages.
Request:
Method: GET
Headers: None
Body: None

Response:
Status: 200 OK (success), 500 (error)
Body (JSON):{
  "status": "operational",
  "embedding_model": "sentence-transformer",
  "supported_languages": ["Bengali", "English"],
  "total_documents": 100
}
Example:curl https://<ngrok-id>.ngrok-free.app/stats

5. Web Interface
Endpoint: GET /chat
Description: Provides a simple HTML interface for testing the chat endpoint.
Request:
Method: GET
Headers: None
Body: None

Response:
Status: 200 OK
Body: HTML page with input field, sample questions, and buttons for health check and stats.
Access: Open https://<ngrok-id>.ngrok-free.app/chat in a browser.

Authentication
Requires an ngrok authtoken for public access. Configure via:configure_ngrok_token('your_token_here')
Obtain token from ngrok dashboard.

Usage
Start Server:start_api_server('/path/to/HSC26_chunks.txt', authtoken='your_token')

Test in Colab:test_api_directly()

HTTP Client:client = SimpleRAGClient('https://<ngrok-id>.ngrok-free.app')
client.ask('Your question here')

Notes
Dataset: Uses /content/drive/MyDrive/dataset/HSC26_chunks.txt for RAG.
Dependencies: Flask, LangChain, Hugging Face Hub, Requests, Pyngrok.

#=== RAG SYSTEM EVALUATION REPORT ===
Generated on: 2025-07-26 08:21:26

📊 AGGREGATE METRICS:
- Average Groundedness Score: 0.677
- Average Relevance Score: 0.849
- Average Answer Accuracy: 0.333

📋 DETAILED RESULTS:

Test Case 1:
Question: অনুপমের ভাষায় সুপুরুষ কাকে বলা হয়েছে?
Expected: শুম্ভনাথ
Got: ‘অপরিচিতা’ মনস্তাপে ভেঙ্গেপড়া এক ব্যক্তীত্বহীন যুবকের স্বীকারোক্তির গল্প, তার পা
Groundedness: 0.700
Relevance: 0.843
Accuracy: 0.428
==================================================

Test Case 2:
Question: কাকে অনুপমের ভাগ্য দেবতা বলে উল্লেখ করা হয়েছে?
Expected: মামো
Got: এ যজ্ঞে পতিনিন্দা শুনে সতী দেহত্যাগ করেন
Groundedness: 0.665
Relevance: 0.861
Accuracy: 0.344
==================================================

Test Case 3:
Question: বিয়ের সময় কল্যাণীর প্রকৃত বয়স কত ছিল?
Expected: ১৫ বছর
Got: এ যজ্ঞে পতিনিন্দা শুনে সতী দেহত্যাগ করেন
Groundedness: 0.665
Relevance: 0.844
Accuracy: 0.226
==================================================


📈 EVALUATION METRICS SUMMARY:
       groundedness_score  relevance_score  accuracy_score
count            3.000000         3.000000        3.000000
mean             0.676643         0.849376        0.332566
std              0.020298         0.010504        0.101521
min              0.664923         0.842681        0.225894
25%              0.664923         0.843323        0.284849
50%              0.664923         0.843966        0.343804
75%              0.682502         0.852724        0.385903
max              0.700081         0.861482        0.428001

#Ouestion Answers
1) What method or library did you use to extract the text, and why? Did you face any formatting challenges with the PDF content?

A: The code uses the tiktoken library with cl100k_base encoding for tokenization. It's chosen for its efficiency, compatibility with GPT-3.5/4 models, and ability to handle multilingual text like Bengali for precise chunking.
Yes, I faced challenges with there pdf contents, I created a bilingual (Bengali/English) document by writing the Bengali content in a separate file, converting it to Markdown, then translating it to English and merging both versions into a single Markdown file.

2) What chunking strategy did you choose (e.g. paragraph-based, sentence-based, character limit)? Why do you think it works well for semantic retrieval? 

A) The code uses a hybrid chunking strategy (paragraph- and sentence-based with a 500-token limit and 50-token overlap). It works well for semantic retrieval by preserving coherent meaning, ensuring model-compatible chunk sizes, maintaining context with overlap, and supporting multilingual text like Bengali.

3) What embedding model did you use? Why did you choose it? How does it capture the meaning of the text?

A) The code uses the sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2 embedding model. It was chosen for its multilingual support, efficiency on CPU, and ability to generate compact, semantically rich embeddings for free usage. It captures text meaning by mapping sentences to a dense vector space, preserving semantic similarity across languages like Bengali and English.

4) How are you comparing the query with your stored chunks? Why did you choose this similarity method and storage setup?

A) The query is compared to stored chunks using cosine similarity in a Chroma vectorstore, retrieving the top 3 similar chunks. Cosine similarity is chosen for its effectiveness in measuring semantic similarity, and Chroma for its scalability and LangChain integration, ideal for bilingual text retrieval.

5) How do you ensure that the question and the document chunks are compared meaningfully? What would happen if the query is vague or missing context?

A) Meaningful Comparison: Uses paraphrase-multilingual-MiniLM-L12-v2 embeddings, cosine similarity in Chroma vectorstore, and top-3 chunk retrieval to match query and chunk semantics, with a prompt template for context-aware answers.

Vague/Missing Context: Vague queries may retrieve irrelevant chunks, leading to generic or incorrect answers. The system falls back to pattern matching or returns "তথ্য পাওয়া যায়নি" if no match is found.

6) Do the results seem relevant? If not, what might improve them (e.g. better chunking, better embedding model, larger document)?

A) Yes, there test results are irrelevant (e.g., expected "শুম্ভনাথ" but got "টা নিতান্ত নির্জীব"). Issues stem from poor retrieval and simplistic answer extraction. To improve:

Refine Chunking: Maybe using better paid version of language models for semantic chunking/ agentic group chunking bring better results.
Better Embedding Model: Adopt intfloat/multilingual-e5-large for enhanced semantic capture.
Improved Extraction: Enhance _extract_answer_from_context with NLP-based matching.























