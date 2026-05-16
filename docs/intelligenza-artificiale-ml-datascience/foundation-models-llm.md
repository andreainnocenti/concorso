# Foundation Models e Large Language Models

## 📖 Introduzione

I **Foundation Models** sono modelli di AI pre-addestrati su enormi quantità di dati che possono essere adattati a molteplici task downstream. I **Large Language Models (LLM)** sono foundation models specializzati nel linguaggio naturale.

**Caratteristiche:**
- **Scale**: Miliardi di parametri (GPT-3: 175B, GPT-4: ~1.7T)
- **Pre-training**: Addestrati su corpus enormi (internet, libri, codice)
- **Versatilità**: Un modello, molti task (zero-shot, few-shot)
- **Emergent abilities**: Capacità che emergono solo a grande scala

---

## 🤖 GPT (Generative Pre-trained Transformer)

### Architettura

GPT usa **Transformer decoder-only** con autoregressive generation.

**Processo:**
1. **Pre-training**: Predice prossima parola su enormi corpus
2. **Fine-tuning**: Adatta a task specifici (opzionale con GPT-3+)

**Obiettivo pre-training:**
```
P(w₁, w₂, ..., wₙ) = P(w₁) × P(w₂|w₁) × P(w₃|w₁,w₂) × ...
```

### Evoluzione GPT

**GPT-1 (2018):**
- 117M parametri
- 12 layer Transformer
- Pre-training su BooksCorpus

**GPT-2 (2019):**
- 1.5B parametri
- Zero-shot learning
- "Too dangerous to release" (inizialmente)

**GPT-3 (2020):**
- 175B parametri
- Few-shot learning senza fine-tuning
- In-context learning

**GPT-4 (2023):**
- ~1.7T parametri (stimato)
- Multimodal (testo + immagini)
- Reasoning migliorato

### In-Context Learning

GPT-3+ può imparare da esempi nel prompt senza training.

**Zero-shot:**
```
Prompt: "Translate to French: Hello"
Output: "Bonjour"
```

**Few-shot:**
```
Prompt: 
"Translate to French:
Hello → Bonjour
Goodbye → Au revoir
Thank you → Merci
Good morning → "

Output: "Bonjour"
```

**Esempio Python:**
```python
import openai

response = openai.ChatCompletion.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Explain quantum computing in simple terms"}
    ]
)

print(response.choices[0].message.content)
```

---

## 📚 BERT (Bidirectional Encoder Representations from Transformers)

### Architettura

BERT usa **Transformer encoder-only** con bidirectional context.

**Differenza chiave vs GPT:**
- GPT: Unidirezionale (sinistra → destra)
- BERT: Bidirezionale (contesto completo)

### Pre-training Tasks

**1. Masked Language Modeling (MLM):**
```
Input: "The [MASK] sat on the mat"
Target: Predici "cat"

15% token mascherati:
- 80%: [MASK]
- 10%: token random
- 10%: token originale
```

**2. Next Sentence Prediction (NSP):**
```
Input: [CLS] Sentence A [SEP] Sentence B [SEP]
Target: B segue A? (Yes/No)
```

### Fine-Tuning BERT

```python
from transformers import BertForSequenceClassification, BertTokenizer, Trainer

# Carica modello pre-addestrato
model = BertForSequenceClassification.from_pretrained('bert-base-uncased', num_labels=2)
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')

# Tokenizza dati
train_encodings = tokenizer(train_texts, truncation=True, padding=True)

# Fine-tune
trainer = Trainer(
    model=model,
    train_dataset=train_dataset,
    eval_dataset=val_dataset
)
trainer.train()
```

### Varianti BERT

**RoBERTa (Robustly Optimized BERT):**
- Rimuove NSP
- Training più lungo
- Batch size più grandi
- Performance migliori

**DistilBERT:**
- 40% più piccolo
- 60% più veloce
- 97% performance di BERT

**ALBERT:**
- Parameter sharing tra layer
- Molto più efficiente

---

## 🎯 Fine-Tuning vs Prompt Engineering

### Fine-Tuning

Addestrare modello su dati specifici del task.

**Vantaggi:**
- ✅ Massima performance
- ✅ Personalizzazione completa

**Svantaggi:**
- ❌ Richiede dati etichettati
- ❌ Costoso computazionalmente
- ❌ Serve per ogni task

**Esempio:**
```python
from transformers import GPT2LMHeadModel, GPT2Tokenizer, Trainer

model = GPT2LMHeadModel.from_pretrained('gpt2')
tokenizer = GPT2Tokenizer.from_pretrained('gpt2')

# Fine-tune su tuo dataset
trainer = Trainer(
    model=model,
    train_dataset=train_dataset,
    args=training_args
)
trainer.train()
```

### Prompt Engineering

Ottimizzare prompt per ottenere output desiderato senza training.

**Tecniche:**

**1. Zero-shot:**
```
"Classify sentiment: This movie was amazing! → "
```

**2. Few-shot:**
```
"Classify sentiment:
This movie was terrible! → Negative
This movie was great! → Positive
This movie was boring! → Negative
This movie was fantastic! → "
```

**3. Chain-of-Thought (CoT):**
```
"Q: Roger has 5 tennis balls. He buys 2 more cans of tennis balls. 
Each can has 3 tennis balls. How many tennis balls does he have now?

A: Let's think step by step:
1. Roger starts with 5 balls
2. He buys 2 cans
3. Each can has 3 balls, so 2 × 3 = 6 balls
4. Total: 5 + 6 = 11 balls

Therefore, Roger has 11 tennis balls."
```

**4. Role Prompting:**
```
"You are an expert Python programmer. Write a function to..."
```

**5. Format Specification:**
```
"Generate a JSON with fields: name, age, city"
```

### Parameter-Efficient Fine-Tuning (PEFT)

Tecniche per fine-tuning con meno risorse.

**LoRA (Low-Rank Adaptation):**
- Addestra solo matrici low-rank
- 0.1% parametri da aggiornare
- Performance simile a full fine-tuning

```python
from peft import LoraConfig, get_peft_model

config = LoraConfig(
    r=8,  # rank
    lora_alpha=32,
    target_modules=["q_proj", "v_proj"],
    lora_dropout=0.1
)

model = get_peft_model(base_model, config)
# Solo ~1% parametri trainable!
```

**Prefix Tuning:**
- Addestra solo "prefix" tokens
- Modello base congelato

**Adapter Layers:**
- Inserisce piccoli layer addestrabili
- Resto del modello congelato

---

## 🔍 RAG (Retrieval-Augmented Generation)

### Concetto

Combina **retrieval** (ricerca informazioni) + **generation** (LLM) per risposte accurate e aggiornate.

**Problema LLM puri:**
- Conoscenza limitata a training data
- Possono "allucinare" (inventare fatti)
- Non aggiornati

**Soluzione RAG:**
```
Query → Retrieve relevant docs → Augment prompt → LLM → Answer
```

### Architettura

```python
from langchain.vectorstores import FAISS
from langchain.embeddings import OpenAIEmbeddings
from langchain.chains import RetrievalQA
from langchain.llms import OpenAI

# 1. Crea vector store da documenti
embeddings = OpenAIEmbeddings()
vectorstore = FAISS.from_documents(documents, embeddings)

# 2. Crea retriever
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

# 3. Crea RAG chain
qa_chain = RetrievalQA.from_chain_type(
    llm=OpenAI(),
    retriever=retriever,
    return_source_documents=True
)

# 4. Query
result = qa_chain({"query": "What is the company policy on remote work?"})
print(result["result"])
print(result["source_documents"])
```

### Processo Dettagliato

**1. Indexing:**
```
Documents → Chunk → Embed → Vector DB
```

**2. Retrieval:**
```
Query → Embed → Similarity search → Top-K docs
```

**3. Augmentation:**
```
Prompt = f"""
Context: {retrieved_docs}

Question: {user_query}

Answer based on context:
"""
```

**4. Generation:**
```
LLM(augmented_prompt) → Answer
```

### Vantaggi RAG

- ✅ Informazioni aggiornate (aggiungi nuovi docs)
- ✅ Riduce allucinazioni (grounded in facts)
- ✅ Citazioni (source documents)
- ✅ Domain-specific senza fine-tuning
- ✅ Più economico di fine-tuning

---

## 🖼️ Vision Transformers (ViT)

### Architettura

Applica Transformer a immagini dividendole in patch.

**Processo:**
```
Immagine 224×224 → Patch 16×16 → 196 patches
Ogni patch → Flatten → Linear projection → Embedding
Embeddings → Transformer Encoder → Classification
```

**Codice:**
```python
from transformers import ViTForImageClassification, ViTFeatureExtractor

model = ViTForImageClassification.from_pretrained('google/vit-base-patch16-224')
feature_extractor = ViTFeatureExtractor.from_pretrained('google/vit-base-patch16-224')

inputs = feature_extractor(images=image, return_tensors="pt")
outputs = model(**inputs)
predicted_class = outputs.logits.argmax(-1)
```

**Vantaggi:**
- ✅ Scala meglio di CNN con più dati
- ✅ Attention interpretabile
- ✅ Transfer learning efficace

**Svantaggi:**
- ❌ Richiede molti dati (o pre-training)
- ❌ Meno inductive bias di CNN

---

## 🎨 Multimodal Models

### CLIP (Contrastive Language-Image Pre-training)

Impara rappresentazioni congiunte di immagini e testo.

**Training:**
```
Batch di (immagine, testo) pairs
Obiettivo: Massimizza similarità coppie corrette
           Minimizza similarità coppie incorrette
```

**Applicazioni:**
```python
from transformers import CLIPProcessor, CLIPModel

model = CLIPModel.from_pretrained("openai/clip-vit-base-patch32")
processor = CLIPProcessor.from_pretrained("openai/clip-vit-base-patch32")

# Zero-shot image classification
inputs = processor(
    text=["a photo of a cat", "a photo of a dog"],
    images=image,
    return_tensors="pt",
    padding=True
)

outputs = model(**inputs)
logits_per_image = outputs.logits_per_image
probs = logits_per_image.softmax(dim=1)
```

**Capacità:**
- Zero-shot image classification
- Image-text retrieval
- Text-to-image search

### DALL-E / Stable Diffusion

Generazione immagini da testo.

**DALL-E 2:**
- Usa CLIP embeddings
- Diffusion model per generazione

**Stable Diffusion:**
```python
from diffusers import StableDiffusionPipeline

pipe = StableDiffusionPipeline.from_pretrained("stabilityai/stable-diffusion-2")
pipe = pipe.to("cuda")

image = pipe(
    "A photo of an astronaut riding a horse on Mars",
    num_inference_steps=50
).images[0]
```

**Applicazioni:**
- Arte generativa
- Design
- Prototipazione
- Content creation

### GPT-4 Vision

LLM multimodal che accetta immagini + testo.

```python
response = openai.ChatCompletion.create(
    model="gpt-4-vision-preview",
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "What's in this image?"},
            {"type": "image_url", "image_url": {"url": image_url}}
        ]
    }]
)
```

---

## 🎯 Domande Tipiche per Esame

### 1. Differenza tra GPT e BERT

**Risposta:**

**GPT (Generative Pre-trained Transformer):**
- **Architettura**: Decoder-only
- **Direzione**: Unidirezionale (sinistra → destra)
- **Training**: Predice prossima parola
- **Uso**: Generazione testo
- **Esempio**: Completamento frasi, chatbot

**BERT (Bidirectional Encoder Representations):**
- **Architettura**: Encoder-only
- **Direzione**: Bidirezionale (contesto completo)
- **Training**: Masked Language Modeling
- **Uso**: Comprensione testo
- **Esempio**: Classificazione, Q&A, NER

**Analogia:**
- GPT: Scrittore (genera testo)
- BERT: Lettore (comprende testo)

### 2. Cos'è il few-shot learning in LLM?

**Risposta:**

Capacità di LLM di imparare task da pochi esempi nel prompt, senza training aggiuntivo.

**Esempio:**
```
Prompt:
"Translate to Spanish:
Hello → Hola
Goodbye → Adiós
Thank you → Gracias
Good morning → "

Output: "Buenos días"
```

**Meccanismo:**
- LLM riconosce pattern negli esempi
- Applica pattern a nuovo input
- Tutto in-context (no weight update)

**Vantaggi:**
- ✅ No training data necessari
- ✅ Veloce (no fine-tuning)
- ✅ Flessibile (cambia task cambiando prompt)

**Limitazioni:**
- ❌ Meno accurato di fine-tuning
- ❌ Limitato da context window
- ❌ Costoso (token nel prompt)

### 3. Cos'è RAG e perché è utile?

**Risposta:**

**RAG (Retrieval-Augmented Generation)** combina ricerca informazioni + LLM per risposte accurate.

**Processo:**
1. **Retrieve**: Cerca documenti rilevanti in knowledge base
2. **Augment**: Aggiungi documenti al prompt
3. **Generate**: LLM genera risposta basata su documenti

**Vantaggi:**

1. **Informazioni aggiornate**: Aggiungi nuovi documenti senza re-training
2. **Riduce allucinazioni**: Risposta grounded in fatti reali
3. **Citazioni**: Può mostrare fonti
4. **Domain-specific**: Usa documenti aziendali senza fine-tuning
5. **Economico**: No costo di fine-tuning

**Esempio uso:**
- Chatbot aziendale con policy interne
- Q&A su documentazione tecnica
- Customer support con knowledge base

### 4. Cos'è il prompt engineering?

**Risposta:**

Arte di scrivere prompt efficaci per ottenere output desiderato da LLM.

**Tecniche:**

**1. Clear instructions:**
```
❌ "Write about AI"
✅ "Write a 200-word introduction to AI for beginners, 
    explaining what it is and 3 real-world applications"
```

**2. Few-shot examples:**
```
"Classify sentiment:
'Great product!' → Positive
'Terrible service' → Negative
'It was okay' → "
```

**3. Chain-of-Thought:**
```
"Solve step-by-step:
Q: If 3 apples cost $6, how much do 5 apples cost?
A: Let's think:
1. Cost per apple = $6 / 3 = $2
2. Cost of 5 apples = 5 × $2 = $10"
```

**4. Role assignment:**
```
"You are an expert Python developer. Review this code..."
```

**5. Output format:**
```
"Generate response in JSON format with keys: name, age, city"
```

**Importanza:** Differenza tra output mediocre e eccellente.

### 5. Differenza tra fine-tuning e RAG

**Risposta:**

Due approcci per adattare LLM a task specifici.

**Fine-Tuning:**
- **Cosa**: Ri-addestra modello su dati specifici
- **Quando**: Task ben definito, molti dati etichettati
- **Pro**: Massima performance, personalizzazione completa
- **Contro**: Costoso, richiede dati, statico (no aggiornamenti facili)
- **Esempio**: Chatbot con stile comunicazione specifico

**RAG:**
- **Cosa**: Recupera documenti rilevanti, augmenta prompt
- **Quando**: Serve accesso a informazioni aggiornate
- **Pro**: Aggiornabile, riduce allucinazioni, economico
- **Contro**: Dipende da qualità retrieval, più lento
- **Esempio**: Q&A su documentazione che cambia spesso

**Quando usare cosa:**
- **Fine-tuning**: Stile/formato specifico, task ripetitivo
- **RAG**: Informazioni fattuali, knowledge base dinamica
- **Entrambi**: Combina per best results (fine-tune + RAG)

---

## 📊 Confronto Foundation Models

| Modello | Parametri | Tipo | Punti di Forza | Uso Principale |
|---------|-----------|------|----------------|----------------|
| **GPT-4** | ~1.7T | Decoder | Generazione, reasoning | Chatbot, coding, writing |
| **GPT-3.5** | 175B | Decoder | Generazione veloce | ChatGPT, API |
| **BERT** | 340M | Encoder | Comprensione | Classificazione, NER |
| **T5** | 11B | Encoder-Decoder | Versatilità | Seq2seq tasks |
| **LLaMA 2** | 7-70B | Decoder | Open source | Research, custom deploy |
| **Claude** | ? | Decoder | Safety, long context | Chatbot, analysis |
| **PaLM 2** | 340B | Decoder | Multilingue, reasoning | Google products |
| **Mistral** | 7B | Decoder | Efficienza | Edge deployment |

---

## 🚀 Tendenze Future

### 1. Modelli Più Grandi e Più Efficienti
- Scaling laws: Performance ∝ parametri
- Mixture of Experts (MoE): Attiva solo subset parametri

### 2. Multimodalità
- Testo + Immagini + Audio + Video
- Unified representations

### 3. Reasoning Migliorato
- Chain-of-Thought nativo
- Tool use (calculator, search, code execution)

### 4. Personalizzazione
- Fine-tuning efficiente (LoRA, QLoRA)
- On-device models (Llama 2, Mistral)

### 5. Safety e Alignment
- RLHF (Reinforcement Learning from Human Feedback)
- Constitutional AI
- Red teaming

---

## 🔗 Collegamenti

- **Precedente:** [Reti Neurali e Deep Learning](reti-neurali-deep-learning.md)
- **Successivo:** [Big Data](big-data.md)
- **Correlato:** [Rappresentazione Conoscenza](rappresentazione-conoscenza.md)
