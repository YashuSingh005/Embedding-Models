# 🧬 Embedding Models: Revision & Quick Notes

> **Core Concept:** An embedding model converts raw text into a dense vector (list of floating-point numbers) that captures semantic meaning. Words or sentences with similar meanings map close to each other in vector space.

---

## 🛠️ 1. Environment & Setup (Google Colab)

* **Secret Management:** Never hardcode API keys. Use Colab's built-in Secret Manager (`🔑 Key icon` on the left sidebar).
* **Naming rule:** Colab secret names cannot contain spaces (e.g., use `Gemini_API_Key`).

---

## 💻 2. Complete Revision Code Template

```python
from google import genai
from google.genai import types
import pandas as pd
from sklearn.metrics.pairwise import cosine_similarity
from IPython.display import display
from google.colab import userdata 

# 1. Secure API Key Retrieval
api_key = userdata.get("Gemini_API_Key")
if not api_key:
  raise RuntimeError(
      "Google_API_KEY NOT FOUND. "
      "ADD IT VIA COLLAB---> SECRET---> ENABLE NOTEBOOK ACCESS..."
  )

# 2. Initialize GenAI Client
client = genai.Client(api_key=api_key)

# 3. Define Test Corpus (Multi-Domain Sentences)
texts = [
    # AI & Software
    "Artificial intelligence and machine learning are transforming modern software development.",
    "Deep learning models require massive amounts of data and computational power.",
    "Kubernetes automates the deployment, scaling, and management of containerized applications.",
    
    # Baking & Food
    "How do I bake a classic chocolate chip cookie from scratch?",
    "The recipe calls for two cups of flour, sugar, and fresh butter.",
    
    # Space & Astronomy
    "SpaceX successfully launched another Falcon 9 rocket into low Earth orbit.",
    "Astronomers discovered a new exoplanet located in the habitable zone of its star.",
    
    # Health & Fitness
    "Regular cardiovascular exercise and weight training improve overall physical health.",
    "Eating a balanced diet rich in protein and vegetables helps with muscle recovery.",
    
    # Cloud Computing
    "Cloud computing platforms like AWS and Google Cloud provide scalable enterprise infrastructure."
]

# 4. Generate Embeddings (with Custom Config)
result = client.models.embed_content(
    model="gemini-embedding-001",
    contents=texts,
    config=types.EmbedContentConfig(
        task_type="SEMANTIC_SIMILARITY", 
        output_dimensionality=768 # Compresses vector size
    )
)

# 5. Inspect Results
for text, embedding in zip(texts, result.embeddings):
    embedding_values = embedding.values
    print(f"Text length: {len(text)}")
    print(f"Embedding vector length: {len(embedding_values)}")
    print(f"Text: {text}\nEmbedding (first 5 vals): {embedding_values[:5]}\n")
