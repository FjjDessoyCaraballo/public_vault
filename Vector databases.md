# What is a vector database?

A vector database is a form of storing different data (audio files, images...) into a high-dimensional database.

![[Pasted image 20251001101056.png]]
Image: representation of what a high-dimensional database would look like

## What is embedding?

Embedding is the transformation of data into significant numerical vector with dense meaning.

![[Pasted image 20251001101038.png]]

## Popular Vector Databases

1. **Pinecone:** Fully managed, cloud native vector database with high scalability and low latency search.
2. **Weaviate**: Open source, supports hybrid (keyword + vector) search and offers built in machine learning modules.
3. **Milvus:** Highly scalable, open source database optimized for large scale similarity search.
4. **Qdrant:** Open source, focuses on high recall, performance and ease of integration with AI applications.
5. **Chromadb:** Lightweight, developer friendly vector database often used in LLM powered applications.

## Applications

1. **Image and Video Search**: Finds visually similar media from a database. Feature embeddings are extracted from media files and stored in the vector database. When a new image or frame is queried, the system quickly retrieves the most visually similar results.
2. **Question Answering Systems:** Retrieves the most relevant information from large knowledge bases. The system embeds both queries and stored text then compares their vectors to find the closest match. This improves accuracy compared to simple keyword matching.
3. **Cross Lingual Information Retrieval**: Supports matching across multiple languages using multilingual embeddings. Text in different languages is converted into a shared embedding space. This allows searching in one language and retrieving relevant results in another.
4. **Fraud and Anomaly Detection**: Identifies unusual patterns by comparing embeddings with normal data. The database can store embeddings of typical behavior and detect deviations. This helps in early identification of fraudulent or suspicious activities.

___
### Sources:

1. https://www.geeksforgeeks.org/dbms/what-is-a-vector-database/
2. https://github.com/facebookresearch/faiss