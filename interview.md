# API Developer Interview Questions - Comprehensive Guide
## (Backend + Generative AI + Agentic AI Focus)

---

## 1. BACKEND & API DEVELOPMENT (Core Foundation)

### REST APIs

**1. Design a REST API for an e-commerce platform. What principles would you follow?**
- **Why asked:** Tests understanding of REST principles, proper HTTP methods, status codes, resource naming
- **Expected answer:** Discuss statelessness, resource-based URLs, proper HTTP verbs (GET/POST/PUT/DELETE/PATCH), meaningful status codes, versioning strategies, error handling
- **Follow-up:** How would you handle pagination? Rate limiting? Caching?

**2. Explain the difference between PUT and PATCH methods. When would you use each?**
- **Why asked:** Shows grasp of HTTP semantics and idempotency
- **Expected answer:** PUT replaces entire resource (idempotent), PATCH partially updates (may or may not be idempotent). PATCH is safer for partial updates
- **Example:** Updating user profile - PATCH for single field change, PUT for complete profile replacement

**3. How do you handle API versioning in REST APIs? What are the trade-offs?**
- **Why asked:** Real-world API evolution challenges
- **Expected answer:** URL versioning (/v1/, /v2/), header versioning, query parameter versioning. Trade-offs: maintenance, backward compatibility, client migration
- **Best practice:** Discuss deprecation strategies and clear versioning roadmap

**4. Design an API endpoint for file uploads. What challenges would you address?**
- **Why asked:** Tests handling of large data, multipart forms, security, storage considerations
- **Expected answer:** Multipart/form-data, chunk uploads for large files, virus scanning, storage optimization, progress tracking, timeout handling
- **Security:** Validate file types, size limits, prevent path traversal

**5. How would you implement pagination for a large dataset in a REST API?**
- **Why asked:** Performance and scalability considerations
- **Expected answer:** Offset-based, cursor-based (better for large datasets), keyset pagination. Discuss pros/cons. Cursor-based is preferred for scale
- **Example:** `/users?cursor=abc123&limit=20`

---

### GraphQL APIs

**6. What are the key advantages of GraphQL over REST? When would you choose GraphQL?**
- **Why asked:** Understands both paradigms and trade-offs
- **Expected answer:** Single endpoint, precise data fetching (no over/under-fetching), strongly typed schema, excellent for mobile/bandwidth-constrained clients, better developer experience
- **Cons:** Caching complexity, query complexity attacks, learning curve, overkill for simple APIs

**7. How would you prevent GraphQL query complexity attacks?**
- **Why asked:** Security awareness in API design
- **Expected answer:** Query depth limits, field count limits, timeout on expensive queries, authentication/rate limiting, query cost analysis, APQ (Automatic Persisted Queries)
- **Tool mention:** apollo-server has built-in plugins for this

**8. Design a GraphQL schema for a real-time chat application.**
- **Why asked:** Schema design skills, understanding subscriptions, complex relationships
- **Expected answer:** Types (Message, User, Conversation), mutations (sendMessage), subscriptions (onNewMessage), proper field resolution, pagination
- **Follow-up:** How would you handle real-time updates efficiently?

**9. Explain N+1 query problem in GraphQL and how to solve it.**
- **Why asked:** Common performance pitfall
- **Expected answer:** Each field resolver can trigger a query. Solution: DataLoader (batching), caching, optimized database queries
- **Example:** Fetching users with posts - without batching, fetching 100 users triggers 100 queries for posts

---

### General API Best Practices

**10. How do you design error responses in APIs? Show an example.**
- **Why asked:** User experience, debugging, standards compliance
- **Expected answer:** Consistent error structure with status code, error code (not just message), message, details. HTTP status codes correctly used
```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "Validation failed",
    "details": {
      "email": "Invalid email format"
    },
    "timestamp": "2024-03-20T10:30:00Z"
  }
}
```

**11. Explain API authentication strategies. When would you use JWT vs OAuth vs API keys?**
- **Why asked:** Security fundamentals
- **Expected answer:** 
  - JWT: Stateless, good for microservices, mobile apps
  - OAuth: Third-party integrations, delegated auth
  - API Keys: Server-to-server, simple use cases
- **Mention:** Refresh token strategies, token expiration, secure storage

**12. How would you implement rate limiting in an API?**
- **Why asked:** Scalability and abuse prevention
- **Expected answer:** Token bucket algorithm, sliding window, Redis-based implementation. Return 429 status code with Retry-After header
- **Metrics:** Per-user, per-IP, per-endpoint rate limits

**13. What's your approach to API documentation? Why is it critical?**
- **Why asked:** Developer experience and maintainability
- **Expected answer:** OpenAPI/Swagger specs, examples, changelog, SDKs, interactive docs (Swagger UI), clear error documentation
- **Tools:** Postman, Swagger, ReDoc, OpenAPI Generator

**14. How would you handle backward compatibility when deprecating an API endpoint?**
- **Why asked:** Real-world maintenance scenarios
- **Expected answer:** Versioning, deprecation headers (Sunset, Deprecation), gradual phase-out, clear communication timeline, client migration tools
- **Example:** Version support matrix - v1 deprecated in 6 months, v2 current, v3 new

---

## 2. NODE.JS / PYTHON / JAVA SPECIFIC QUESTIONS

### Node.js Backend

**15. Design a scalable Node.js backend architecture. What would you use for different layers?**
- **Why asked:** System design and framework knowledge
- **Expected answer:** 
  - Web framework: Express, Fastify, NestJS
  - Database: PostgreSQL, MongoDB
  - Cache: Redis
  - Message queue: RabbitMQ, Kafka
  - Monitoring: Prometheus, ELK stack
- **Mention:** Microservices vs monolith trade-offs

**16. How do you handle asynchronous operations in Node.js? Callbacks vs Promises vs Async/Await?**
- **Why asked:** Core JavaScript knowledge
- **Expected answer:** Evolution from callbacks (callback hell) → Promises → Async/Await. Async/Await is most readable, error handling with try/catch
- **Example:**
```javascript
// Async/Await
try {
  const user = await getUserFromDB(id);
  const posts = await getPostsForUser(user.id);
  return { user, posts };
} catch (err) {
  console.error('Error:', err);
}
```

**17. Explain event-driven architecture in Node.js. When would you use EventEmitter?**
- **Why asked:** Understanding Node.js paradigm
- **Expected answer:** Decoupled components, pub-sub pattern, good for streaming, middleware. EventEmitter for custom events
- **Use case:** User registration triggers multiple events (email, analytics, notification)

**18. How would you structure a large Node.js project?**
- **Why asked:** Code organization and maintainability
- **Expected answer:** 
```
src/
  controllers/
  services/
  models/
  routes/
  middleware/
  utils/
  config/
tests/
```
- **Mention:** Separation of concerns, DI containers, testing approach

**19. What's the difference between `require()` and `import`? When would you use each?**
- **Why asked:** Modern Node.js practices
- **Expected answer:** require (CommonJS, synchronous), import (ES6 modules, asynchronous). ESM is future standard. `import` preferred for new projects
- **Note:** Node.js now supports both; discuss when to use which

---

### Python Backend

**20. Design a Python FastAPI/Django application structure. What are the key components?**
- **Why asked:** Framework knowledge and architecture
- **Expected answer:** Routes, controllers, services, models, middleware, error handling. FastAPI is async-first, Django is batteries-included
- **Comparison:** FastAPI for APIs (async, modern), Django for full-stack apps (ORM, admin panel)

**21. Explain decorators in Python. How would you use them in FastAPI?**
- **Why asked:** Python-specific advanced concept
- **Expected answer:** Decorators modify function behavior. In FastAPI: `@app.get()`, `@app.post()`, custom decorators for auth/validation
```python
def require_auth(func):
    def wrapper(*args, **kwargs):
        # Validate token
        return func(*args, **kwargs)
    return wrapper
```

**22. How do you handle dependency injection in Python?**
- **Why asked:** Testing and maintainability
- **Expected answer:** FastAPI has built-in DI system, manual DI containers, or libraries like `injector`. Discuss benefits for testing
- **Example:** Mocking database in tests using DI

**23. Explain async/await in Python. How is it different from Node.js?**
- **Why asked:** Concurrency model understanding
- **Expected answer:** Both use async/await syntax. Python uses event loop (asyncio), must explicitly await. Node.js is single-threaded event loop by default
- **Threading:** Python has GIL (Global Interpreter Lock), can use multiprocessing for CPU-bound tasks

---

### Java Backend

**24. Explain Spring Boot architecture. How would you build a REST API in Spring?**
- **Why asked:** Enterprise-level backend knowledge
- **Expected answer:** IoC container, annotations (@RestController, @Service, @Repository), dependency injection, autoconfiguration
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        // Implementation
    }
}
```

**25. What's the difference between @Service, @Repository, and @Component annotations?**
- **Why asked:** Spring framework semantics
- **Expected answer:** 
  - @Service: Business logic layer
  - @Repository: Data access layer (with exception translation)
  - @Component: Generic Spring-managed component
- **When to use:** Different semantic meanings and exception handling

**26. How would you handle transactions in Spring? Discuss @Transactional.**
- **Why asked:** Data consistency and reliability
- **Expected answer:** ACID properties, `@Transactional` annotation, propagation levels (REQUIRED, REQUIRES_NEW, etc.), rollback rules
- **Example:** Multi-step operation where all succeed or all fail

**27. Explain Spring Security. How would you implement JWT authentication?**
- **Why asked:** Security implementation
- **Expected answer:** Authentication, authorization, FilterChain, JWT token generation/validation, custom security filters
- **Flow:** Client sends JWT in Authorization header, server validates and extracts claims

---

## 3. LLM INTEGRATION & GENERATIVE AI

### LLM APIs Fundamentals

**28. You need to integrate OpenAI's GPT API into your application. Walk me through your approach.**
- **Why asked:** Hands-on LLM integration skills
- **Expected answer:**
  1. API key management (environment variables, secret manager)
  2. Client initialization
  3. Error handling (rate limits, API failures)
  4. Token counting (cost optimization)
  5. Streaming vs non-streaming responses
  6. Timeout handling
```python
from openai import OpenAI

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Your prompt"}],
    max_tokens=500,
    temperature=0.7
)
```

**29. What are tokens in LLMs? How would you estimate costs and manage token usage?**
- **Why asked:** Cost management and optimization
- **Expected answer:** Tokens are subwords. Use token counters (tiktoken), monitor token usage, implement caching for repeated prompts, batch requests
- **Cost calculation:** Input tokens cost less than output tokens. GPT-4 ~0.03/1k input, ~0.06/1k output
- **Optimization:** Fewer instructions, shorter context windows, use cheaper models (GPT-3.5) where appropriate

**30. Compare different LLM providers: OpenAI, Gemini, Anthropic, Llama, Mistral. When would you choose each?**
- **Why asked:** Understanding LLM landscape
- **Expected answer:**
  - **OpenAI (GPT-4):** Best performance, expensive, good for complex tasks
  - **Gemini:** Multimodal, good context window, competitive pricing
  - **Anthropic (Claude):** Long context (200k tokens), strong reasoning, safe AI practices
  - **Llama (Meta):** Open-source, can self-host, cost-effective, active community
  - **Mistral:** Efficient, fast inference, good for real-time applications
- **Decision factors:** Cost, latency, context window, specific capabilities needed

**31. How would you handle LLM API failures and rate limiting?**
- **Why asked:** Production reliability
- **Expected answer:** 
  - Exponential backoff with jitter
  - Fallback to cheaper/faster models
  - Request queuing with rate limiter
  - Caching responses
  - Circuit breaker pattern
  - Monitoring and alerting
```python
import time
import random

def call_with_retry(prompt, max_retries=3):
    for attempt in range(max_retries):
        try:
            return client.chat.completions.create(...)
        except RateLimitError:
            wait_time = (2 ** attempt) + random.random()
            time.sleep(wait_time)
```

**32. What is prompt engineering? Give examples of effective vs ineffective prompts.**
- **Why asked:** Maximizing LLM output quality
- **Expected answer:** Art of crafting prompts for better results. Ineffective: vague, ambiguous. Effective: specific, context, role, constraints
```
❌ Bad: "Write about AI"
✅ Good: "You are a technical writer. Write a 200-word beginner's guide to transformers in deep learning, explaining attention mechanisms without math jargon."
```
- **Techniques:** Few-shot learning, chain-of-thought, role-playing, constraints

**33. Explain few-shot prompting and chain-of-thought. When would you use each?**
- **Why asked:** Advanced prompting techniques
- **Expected answer:**
  - **Few-shot:** Provide examples before asking task. Great for classification, formatting
  - **Chain-of-thought:** Encourage step-by-step reasoning. Improves complex problem solving
```
Few-shot example:
Input: "Apple Inc"
Output: {"name": "Apple", "sector": "Technology"}

Chain-of-thought:
"Let's think step by step: First, identify the components. Second, analyze relationships. Third, derive conclusion."
```

**34. How would you build a chatbot that remembers conversation history?**
- **Why asked:** Stateful AI interactions
- **Expected answer:** 
  - Store conversation history in database/memory
  - Include relevant history in each API call
  - Manage context window (don't exceed token limits)
  - Summarization for long conversations
  - User session management
```python
messages = [
    {"role": "system", "content": "You are helpful assistant"},
    {"role": "user", "content": "Hi"},
    {"role": "assistant", "content": "Hello! How can I help?"},
    {"role": "user", "content": "What's 2+2?"}  # New message
]
response = client.chat.completions.create(model="gpt-4", messages=messages)
```

**35. What are embeddings? How would you use them in your application?**
- **Why asked:** Foundation for semantic search, similarity, and RAG
- **Expected answer:** Dense vectors representing semantic meaning. Same semantic meaning = similar vectors. Use for:
  - Semantic search (query embeddings, find similar docs)
  - Clustering similar content
  - Recommendation systems
  - Building RAG pipelines
```python
# Generate embedding
embedding = client.embeddings.create(
    model="text-embedding-3-small",
    input="The quick brown fox"
)

# Compare similarity using cosine distance
from sklearn.metrics.pairwise import cosine_similarity
similarity = cosine_similarity([embedding1], [embedding2])
```

**36. Explain multimodal LLMs. How would you integrate image processing with GPT-4V?**
- **Why asked:** Understanding modern LLM capabilities
- **Expected answer:** Process text + images together. Use GPT-4V, Gemini Vision. Applications: document analysis, visual Q&A, OCR
```python
response = client.chat.completions.create(
    model="gpt-4-vision-preview",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "What's in this image?"},
                {
                    "type": "image_url",
                    "image_url": {"url": "https://...image.jpg"}
                }
            ]
        }
    ]
)
```

---

## 4. VECTOR DATABASES & RAG (Retrieval-Augmented Generation)

### Vector Database Fundamentals

**37. What is a vector database? Why is it different from traditional databases?**
- **Why asked:** Core concept for modern AI applications
- **Expected answer:** Stores and searches dense vectors using distance metrics (cosine, L2, dot product). Enables semantic search. Traditional DBs use exact matching
- **Key advantage:** Find semantically similar items even if not exact matches

**38. Compare popular vector databases: Pinecone, Weaviate, Milvus, Qdrant, Chroma.**
- **Why asked:** Practical technology selection
- **Expected answer:**
  - **Pinecone:** Fully managed, easy to use, expensive, good for MVPs
  - **Weaviate:** Open-source, self-hosted, good balance, GraphQL interface
  - **Milvus:** Open-source, high-performance, cloud-native, complex setup
  - **Qdrant:** Rust-based, fast, good API, good for production
  - **Chroma:** Lightweight, embedded, great for prototyping/small apps
- **Decision factors:** Scale, cost, operational complexity, latency requirements

**39. How would you design a semantic search system using embeddings and vector database?**
- **Why asked:** Practical implementation of vector DB
- **Expected answer:**
  1. Generate embeddings for documents (batch process)
  2. Store in vector DB with metadata
  3. For search query, generate embedding
  4. Query vector DB with similarity search
  5. Rank and return top-k results with scores
```python
# Index documents
for doc in documents:
    embedding = generate_embedding(doc.content)
    vector_db.upsert(id=doc.id, vector=embedding, metadata={"title": doc.title})

# Search
query_embedding = generate_embedding("user query")
results = vector_db.query(query_embedding, top_k=10)
```

**40. What is cosine similarity? Why is it commonly used for vector similarity?**
- **Why asked:** Understanding similarity metrics
- **Expected answer:** Measures angle between vectors (0-1 scale), ranges 0-1 where 1 is identical. Invariant to vector magnitude, works well for normalized embeddings
- **Formula:** cos(θ) = (A·B)/(||A|| ||B||)
- **When to use:** Embeddings (normalized vectors). Other metrics: L2 (Euclidean), dot product (prenormalized)

**41. How would you handle updates and deletions in a vector database at scale?**
- **Why asked:** Practical database operations
- **Expected answer:** Vector DBs typically support soft deletes, use version control, async deletion for large batches, consider eventual consistency
- **Strategy:** Mark as deleted, periodic cleanup, sync with source of truth

---

### RAG (Retrieval-Augmented Generation)

**42. What is RAG and why is it important? How does it improve upon vanilla LLMs?**
- **Why asked:** Understanding modern AI architecture
- **Expected answer:** RAG = retrieve relevant documents + generate with context. Solves:
  - Knowledge cutoff (always uses latest data)
  - Hallucination (grounds responses in facts)
  - Domain-specific knowledge (can teach LLM about private data)
  - Citations (can show sources)
- **Process:** Query → Retrieve relevant docs → Augment prompt → Generate

**43. Design a RAG system for a customer support chatbot with product documentation.**
- **Why asked:** Real-world RAG application
- **Expected answer:**
  1. Index all product docs as vectors
  2. User question → generate embedding
  3. Retrieve top-k relevant docs from vector DB
  4. Augment prompt with retrieved docs
  5. Generate response with context
  6. Include source citations
```python
# Pseudocode
user_query = "How do I reset my password?"
docs = retrieve_from_vector_db(user_query, top_k=5)
context = format_docs(docs)
prompt = f"Context: {context}\n\nQuestion: {user_query}"
response = generate_with_llm(prompt)
```
- **Advanced:** Hybrid search (keyword + semantic), re-ranking, query expansion

**44. What are common failure modes in RAG systems? How would you debug them?**
- **Why asked:** Production troubleshooting
- **Expected answer:**
  - **Poor retrieval:** Wrong docs fetched → improve embeddings, tune similarity threshold, use hybrid search
  - **Too much context:** Token limit exceeded → use better ranking, chunk better
  - **Hallucination:** LLM ignoring docs → stricter prompts, use function calling
  - **Stale data:** Outdated retrieved docs → implement refresh strategy
- **Debugging:** Log retrieved docs, review similarity scores, test retrieval independently

**45. How would you chunk long documents for RAG? What are the trade-offs?**
- **Why asked:** Practical document processing
- **Expected answer:** Chunk size impacts retrieval quality. Too small = fragmented context, too large = includes noise
- **Strategies:**
  - Fixed size (simple, may split sentences)
  - Semantic chunking (chunk by meaning, better but complex)
  - Recursive chunking (hierarchical)
- **Best practice:** 256-1024 tokens per chunk, overlap 20% to maintain context
```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=800,
    chunk_overlap=200
)
chunks = splitter.split_text(document)
```

**46. Explain the difference between retrieval and re-ranking in RAG.**
- **Why asked:** Optimization techniques
- **Expected answer:** 
  - **Retrieval:** Fast similarity search (coarse-grained)
  - **Re-ranking:** Accurate relevance scoring on retrieved set (fine-grained)
- **Two-stage approach:** Retrieve 100 documents (fast), re-rank top 10 (accurate)
- **Tools:** Cross-encoders for re-ranking (more accurate than bi-encoders)

**47. How would you measure RAG system quality?**
- **Why asked:** Evaluation and monitoring
- **Expected answer:** 
  - **Retrieval metrics:** Precision, Recall, NDCG, MAP
  - **Generation metrics:** BLEU, ROUGE, F1 (information presence)
  - **User metrics:** Relevance rating, answer correctness, citation accuracy
  - **Business metrics:** User satisfaction, support ticket reduction, resolution time
- **Mention:** Human evaluation for quality assurance, automated evals for scale

---

## 5. AGENTIC AI & MULTI-STEP WORKFLOWS

### Agent Fundamentals

**48. What is an Agentic AI system? How does it differ from a chatbot?**
- **Why asked:** Core concept of modern AI systems
- **Expected answer:** 
  - **Chatbot:** Responds to user input, stateless, follows prompts
  - **Agent:** Autonomous decision-maker, uses tools, iterative reasoning, maintains state
- **Key capabilities:** Tool use, planning, memory, self-correction, monitoring
- **Example:** ChatBot answers questions about orders; Agent can check status, create return, contact support

**49. Explain the ReAct (Reasoning + Acting) pattern.**
- **Why asked:** Understanding agent reasoning
- **Expected answer:** Agent thinks through steps (Thought), chooses action (Act), observes result (Observation), repeats. More reliable than pure chain-of-thought
- **Loop:**
  ```
  Thought: I need to find product price
  Action: search_database(product_id)
  Observation: Found price = $99
  Thought: Can calculate total with discount
  Action: calculate_discount(price, discount_rate)
  ```

**50. Design an autonomous customer service agent that handles refunds, replacements, and escalations.**
- **Why asked:** Complex multi-step agent design
- **Expected answer:**
  1. **Understand request** (LLM classifies issue type)
  2. **Retrieve context** (customer history, order details)
  3. **Reason about options** (is customer eligible for refund?)
  4. **Execute actions** (process refund, send confirmation email)
  5. **Monitor outcomes** (verify success, escalate if needed)
- **Tools needed:** 
     - lookup_customer_history
     - check_refund_policy
     - process_refund
     - create_support_ticket
     - send_email
- **Error handling:** If action fails, try alternative, escalate if no solution found

---

### LangChain, LlamaIndex, AutoGen

**51. What is LangChain? How would you use it to build an agent?**
- **Why asked:** Popular agent framework
- **Expected answer:** Framework for building LLM applications. Components: Models, Chains, Tools, Memory, Agents
- **Agent types:** ReAct, OpenAI Functions, Conversational, Tool-using
```python
from langchain.agents import initialize_agent, Tool
from langchain.llms import OpenAI

tools = [
    Tool(name="Search", func=search_api, description="Search for info"),
    Tool(name="Calculator", func=calculate, description="Perform math")
]

agent = initialize_agent(tools, llm=OpenAI(), agent="zero-shot-react-description")
result = agent.run("What's 2+2? And tell me about AI.")
```

**52. Explain LangChain chains. Design a multi-step chain for generating and validating code.**
- **Why asked:** Chain composition and orchestration
- **Expected answer:** Chains link components together. Sequential, parallel, conditional chains
```python
from langchain.chains import LLMChain, SequentialChain

# Step 1: Generate code
generate_chain = LLMChain(llm=llm, prompt=generate_prompt)

# Step 2: Validate code
validate_chain = LLMChain(llm=llm, prompt=validate_prompt)

# Step 3: Explain code
explain_chain = LLMChain(llm=llm, prompt=explain_prompt)

# Combine sequentially
overall_chain = SequentialChain(
    chains=[generate_chain, validate_chain, explain_chain],
    input_variables=["requirement"]
)
```

**53. What is memory in LangChain? How would you implement conversation memory?**
- **Why asked:** Maintaining context in agents
- **Expected answer:** Memory stores conversation history. Types:
  - **Buffer:** Stores all messages (simple, hits token limit)
  - **Summary:** Summarizes old messages (token efficient)
  - **Entity:** Tracks key entities (useful for domain knowledge)
```python
from langchain.memory import ConversationSummaryMemory

memory = ConversationSummaryMemory(llm=llm)
# Automatically summarizes when memory grows

# Or entity memory
from langchain.memory import ConversationEntityMemory
entity_memory = ConversationEntityMemory(llm=llm)
```

**54. Explain LlamaIndex (formerly GPT Index). When would you use it over LangChain?**
- **Why asked:** Understanding tool specializations
- **Expected answer:** LlamaIndex specialized for indexing and retrieval (better for RAG). LangChain is broader agent framework
- **LlamaIndex strengths:** 
  - Better document indexing strategies
  - Built-in RAG optimization
  - Flexible query engines
  - Better for document QA
- **When to use:** Primary goal is RAG → LlamaIndex; Building complex agents → LangChain
- **Hybrid:** Can use both (LlamaIndex for retrieval, LangChain for agent orchestration)

**55. Build a document QA agent using LlamaIndex. Explain architecture.**
- **Why asked:** RAG implementation with indexing framework
- **Expected answer:**
  1. Load documents (PDFs, web pages, etc.)
  2. Create vector index
  3. Build query engine
  4. Create agent with query engine as tool
```python
from llama_index import VectorStoreIndex, SimpleDirectoryReader

documents = SimpleDirectoryReader('data').load_data()
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()

agent = initialize_agent([
    Tool(
        name="document_qa",
        func=query_engine.query,
        description="Answer questions about documents"
    )
], llm)
```

**56. What is AutoGen? How does it enable multi-agent conversations?**
- **Why asked:** Understanding multi-agent systems
- **Expected answer:** Microsoft's framework for multi-agent LLM conversations. Agents have roles, communicate, solve problems collaboratively
- **Key features:** 
  - Customizable agents with different roles
  - Message passing
  - Code execution capability
  - Conversation history management
- **Use case:** Two agents debating solution, one writes code, one reviews
```python
from autogen import AssistantAgent, UserProxyAgent

coder = AssistantAgent(name="Coder", system_message="You write Python code")
reviewer = UserProxyAgent(name="Reviewer", human_input_mode="NEVER")

reviewer.initiate_chat(
    coder,
    message="Write a function to sort a list"
)
```

**57. Design a multi-agent system for data analysis: one agent writes SQL, another validates, another explains results.**
- **Why asked:** Complex multi-agent orchestration
- **Expected answer:**
  - **Agent 1 (Analyst):** Understands query, writes SQL
  - **Agent 2 (Validator):** Checks SQL syntax, reviews logic
  - **Agent 3 (Explainer):** Interprets results for non-technical users
- **Flow:** User query → Analyst writes SQL → Validator reviews → Execute → Explainer summarizes
- **Tools:** Database access, SQL validator, result formatter
- **Error handling:** Retry mechanism, human escalation

---

### Tool/Function Calling

**58. What is function calling (tool calling) in LLMs? Why is it important?**
- **Why asked:** Critical for agent actions
- **Expected answer:** LLM chooses which functions to call based on task. Structured way to invoke backend code from LLM
- **Benefits:** 
  - Deterministic action execution
  - Prevents hallucination of tool outputs
  - Enables complex reasoning + action loops
- **Difference from prompting:** Instead of asking LLM to generate code, it calls actual functions

**59. Design a function calling system for a shopping assistant with tools: search_products, add_to_cart, apply_coupon.**
- **Why asked:** Practical function calling implementation
- **Expected answer:**
  1. Define function schemas (name, description, parameters)
  2. LLM receives tools, decides which to call
  3. Parse function call, execute it
  4. Return result to LLM
  5. LLM uses result for next action
```python
from openai import OpenAI

tools = [
    {
        "name": "search_products",
        "description": "Search product catalog",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {"type": "string", "description": "Search term"},
                "price_range": {"type": "array", "items": {"type": "number"}}
            },
            "required": ["query"]
        }
    }
]

response = client.chat.completions.create(
    model="gpt-4",
    tools=tools,
    messages=[{"role": "user", "content": "Find laptops under $1000"}]
)

# Process tool calls in response
if response.tool_calls:
    for call in response.tool_calls:
        result = execute_function(call.function.name, call.function.arguments)
```

**60. Explain parallel function calling. When would you use it?**
- **Why asked:** Optimization for multi-step tasks
- **Expected answer:** Call multiple functions simultaneously instead of sequentially. Reduces latency when functions don't depend on each other
- **Example:** Fetch user profile, order history, and recommendations in parallel instead of sequentially
- **Tradeoff:** Increases complexity, useful when latency matters

**61. How would you handle errors in function execution? Design retry logic with function calling.**
- **Why asked:** Production reliability
- **Expected answer:** Graceful error handling, inform LLM of failure, allow retry with different function
```python
for attempt in range(max_retries):
    try:
        result = execute_function(func_name, args)
        break
    except SpecificError as e:
        if attempt < max_retries - 1:
            # Inform LLM of error
            messages.append({
                "role": "user",
                "content": f"Function {func_name} failed: {str(e)}. Try alternative."
            })
        else:
            raise
```

---

## 6. SYSTEM DESIGN & ARCHITECTURE

**62. Design a scalable AI chatbot service. Explain the architecture and technology choices.**
- **Why asked:** Full-stack system design thinking
- **Expected answer:** 
```
Frontend → API Gateway → Load Balancer
    ↓
  [Web Server Layer] (multiple instances)
    ↓
[Cache Layer] - Redis for conversation history, rate limiting
    ↓
[Business Logic] - Conversation manager, LLM integration, tool executor
    ↓
[Data Layer] - PostgreSQL (history), Vector DB (embeddings)
    ↓
[External Services] - LLM API, payment gateway, notification service
```
- **Considerations:** Async processing (Celery/RabbitMQ), monitoring, auto-scaling, cost optimization
- **Database schema:** User sessions, conversation history, message metadata

**63. How would you design a system that integrates with multiple LLM providers with automatic failover?**
- **Why asked:** Production resilience
- **Expected answer:**
  1. Provider abstraction layer
  2. Primary provider with fallback chain
  3. Health check monitoring
  4. Cost/latency tracking per provider
  5. Automatic switching on failure
```python
class LLMProvider:
    async def call(self, prompt):
        providers = [
            OpenAIProvider(),
            AnthropicProvider(),  # Fallback
            GeminiProvider()      # Fallback
        ]
        
        for provider in providers:
            if provider.is_healthy():
                try:
                    return await provider.call(prompt)
                except Exception as e:
                    mark_provider_unhealthy(provider)
        
        raise AllProvidersFailedError()
```

**64. Design an autonomous agent system for data processing. Describe the control flow and error handling.**
- **Why asked:** Agent architecture and resilience
- **Expected answer:** Mention state machines, decision trees, human-in-loop for critical decisions
- **Control flow:** Input → Intent Classification → Plan Generation → Action Execution → Verification → Output
- **Error handling:** Validation at each step, rollback capability, escalation thresholds

**65. How would you monitor and observe an AI application? What metrics matter?**
- **Why asked:** Production operations
- **Expected answer:**
  - **System metrics:** Latency, throughput, error rate, availability
  - **AI-specific:** LLM API cost, token usage, temperature settings, model performance
  - **Quality metrics:** Hallucination rate, user satisfaction, tool execution success rate
  - **Tools:** DataDog, Prometheus, custom dashboard
- **Alerting:** High latency, high error rate, cost threshold, failing tools

**66. Design a caching strategy for an AI application. When would you cache LLM responses?**
- **Why asked:** Cost optimization and performance
- **Expected answer:**
  - **Cache layer:** Redis for fast retrieval
  - **Cache key:** Hash of model, temperature, prompt (not exact match to catch variations)
  - **TTL:** Short for volatile data (prices), longer for stable (FAQ answers)
  - **Invalidation:** Active (time-based), passive (version-based)
- **Tradeoff:** Caching helps costs but risks stale answers. Use for deterministic queries only

---

## 7. CLOUD DEPLOYMENT (AWS / AZURE / GCP)

**67. How would you deploy a Node.js API to AWS? Walk through the process.**
- **Why asked:** Cloud deployment experience
- **Expected answer:** 
  1. Package application (Docker)
  2. Push to ECR (Elastic Container Registry)
  3. Deploy via ECS/EKS (container orchestration)
  4. Configure load balancer (ALB)
  5. Setup RDS (database)
  6. Configure CloudWatch (monitoring)
  7. Setup CI/CD pipeline (CodePipeline)
- **Alternative:** Lambda for serverless (if stateless APIs), Elastic Beanstalk (simplest)

**68. Explain containerization. Design a Dockerfile for a Python FastAPI + LLM integration app.**
- **Why asked:** Modern deployment practices
- **Expected answer:**
```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV OPENAI_API_KEY=${OPENAI_API_KEY}
ENV PYTHONUNBUFFERED=1

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```
- **Best practices:** Multi-stage builds, minimal images, security scanning, layer caching

**69. How would you set up CI/CD for a backend API? What should the pipeline include?**
- **Why asked:** Modern development practices
- **Expected answer:** 
  ```
  Trigger (push) → Build → Test → Code Quality → Security Scan → Deploy Staging → Integration Tests → Deploy Prod
  ```
- **Tools:** GitHub Actions, GitLab CI, Jenkins, GitOps (ArgoCD)
- **Tests:** Unit, integration, end-to-end, performance, security

**70. Explain serverless architecture. When would you use Lambda vs containers?**
- **Why asked:** Deployment trade-offs
- **Expected answer:**
  - **Lambda:** Pay-per-execution, auto-scaling, no server management, cold starts (latency)
  - **Containers:** Persistent resources, better for always-on, more control, cost predictable
- **When Lambda:** Sporadic load, webhook handlers, scheduled tasks
- **When Containers:** Real-time APIs, complex long-running tasks, specific dependencies
- **For LLM apps:** Usually containers (consistent latency needed), or Lambda with warm pools

**71. How would you handle secrets management in cloud deployment?**
- **Why asked:** Security best practices
- **Expected answer:** 
  - Never hardcode secrets
  - Use managed services: AWS Secrets Manager, Azure Key Vault, GCP Secret Manager
  - Implement secret rotation
  - Audit access logs
  - Use IAM roles instead of credentials where possible
- **Implementation:** Read secrets at runtime, inject via environment variables

**72. Design a multi-region deployment strategy for an AI service.**
- **Why asked:** Scalability and resilience
- **Expected answer:** 
  - **Primary region:** Low latency for main users
  - **Secondary regions:** Disaster recovery, regional data residency
  - **Data sync:** Eventually consistent (vector DB), or strongly consistent (transaction DB)
  - **Routing:** GeoDNS or application-level based on user location
  - **Costs:** More expensive, weigh against downtime risk

---

## 8. DATABASE & DATA STORAGE

**73. Design a database schema for a conversational AI application. Explain relationships.**
- **Why asked:** Data modeling
- **Expected answer:**
```sql
Users:
  - id (PK)
  - email
  - created_at

Conversations:
  - id (PK)
  - user_id (FK → Users)
  - title
  - created_at

Messages:
  - id (PK)
  - conversation_id (FK → Conversations)
  - role (user/assistant)
  - content
  - tokens_used
  - model_version
  - created_at

Tools_Executed:
  - id (PK)
  - message_id (FK → Messages)
  - tool_name
  - input_params (JSON)
  - output (JSON)
  - execution_time_ms
```
- **Mention:** Indexes, foreign keys, denormalization for performance

**74. How would you optimize a database query that's becoming slow as data grows?**
- **Why asked:** Query optimization
- **Expected answer:**
  1. Profile/identify slow query (EXPLAIN PLAN)
  2. Add indexes on filter/join columns
  3. Avoid N+1 queries
  4. Denormalize if reads dominate
  5. Partition large tables
  6. Cache results
```sql
-- Slow: No index on user_id
SELECT * FROM messages WHERE user_id = 123 AND created_at > NOW() - INTERVAL '7 days';

-- Better: Add composite index
CREATE INDEX idx_messages_user_created ON messages(user_id, created_at DESC);
```

**75. When would you use NoSQL vs SQL? Design both schemas for a product recommendation system.**
- **Why asked:** Database selection decision
- **Expected answer:**
  - **SQL (PostgreSQL):** Structured, relational, ACID, complex queries
  - **NoSQL (MongoDB):** Flexible schema, horizontal scaling, document-oriented
- **Recommendation system:**
  - **SQL approach:** Users → Products → Ratings (normalized)
  - **NoSQL approach:** User documents with embedded preferences array
- **Hybrid:** Use both - PostgreSQL for transactions, MongoDB for user behavior logs

---

## 9. TESTING & QUALITY ASSURANCE

**76. How would you test an API that depends on external LLM APIs?**
- **Why asked:** Testing challenges with external dependencies
- **Expected answer:**
  - **Mock LLM responses:** Use fixtures or mocking library
  - **Isolation:** Don't call real API in unit tests (expensive, slow)
  - **Integration tests:** Test real API in separate test suite (use cheaper models, limit calls)
  - **Fixture data:** Pre-recorded LLM responses for consistency
```python
from unittest.mock import patch
import pytest

@patch('openai.ChatCompletion.create')
def test_chatbot_response(mock_llm):
    mock_llm.return_value = MockResponse("Hello!")
    response = chatbot.ask("Hi")
    assert response == "Hello!"
```

**77. Design a test suite for a RAG system. What would you test?**
- **Why asked:** Testing AI systems
- **Expected answer:**
  - **Retrieval tests:** Correct documents retrieved, relevance scores, edge cases
  - **Generation tests:** Response contains factual info from retrieved docs, citations present
  - **Integration tests:** End-to-end query → retrieval → generation
  - **Regression tests:** Monitor quality over time with metrics
- **Test data:** Labeled QA pairs with expected retrieved documents

**78. How would you handle flakiness in tests that use LLMs?**
- **Why asked:** LLMs produce variable outputs
- **Expected answer:**
  - Use deterministic mocks for unit tests
  - Set temperature=0 for reproducibility in integration tests
  - Use semantic similarity for assertions (not exact string match)
  - Retry tests that are intermittently failing
```python
from nltk.tokenize import word_tokenize
from sklearn.feature_extraction.text import TfidfVectorizer

# Instead of exact match
assert response == expected  # ❌ Flaky

# Use semantic similarity
similarity = cosine_similarity(vectorize(response), vectorize(expected))
assert similarity > 0.8  # ✅ Better
```

**79. How would you load test an API? What LLM-specific challenges exist?**
- **Why asked:** Performance under scale
- **Expected answer:**
  - Load testing tools: JMeter, Locust, k6
  - Simulate concurrent users
  - Monitor latency, throughput, errors
- **LLM challenges:** 
  - External API rate limits (cost!)
  - Variable response time (can't predict latency)
  - Token counting for cost prediction
```python
from locust import HttpUser, task

class ChatbotUser(HttpUser):
    @task
    def ask_question(self):
        self.client.post("/chat", json={"message": "What's 2+2?"})
```

---

## 10. PROBLEM-SOLVING & COMMUNICATION

**80. Tell me about a time you built an API that failed. What went wrong and how did you fix it?**
- **Why asked:** Learning from mistakes, problem-solving process
- **Expected answer:** Specific situation, root cause analysis, solution implemented, prevention going forward
- **Structure:** STAR method (Situation, Task, Action, Result)
- **Good example:** "API returned 5xx errors under load. Investigated and found N+1 query problem. Added caching layer. Reduced latency 10x."

**81. You're integrating a new LLM provider and latency is 2x expected. What's your debugging approach?**
- **Why asked:** Troubleshooting methodology
- **Expected answer:**
  1. Measure end-to-end: network time, LLM API time, processing time
  2. Identify bottleneck (is it their API or our code?)
  3. Check API documentation for optimizations
  4. Consider model choice (smaller model = faster)
  5. Implement caching or batch processing
  6. Monitor with detailed logging
- **Tools:** APM tools, distributed tracing, profilers

**82. A RAG system is hallucinating and returning incorrect information. How would you debug?**
- **Why asked:** Real-world AI system troubleshooting
- **Expected answer:**
  1. Isolate the issue: Is retrieval returning bad docs or LLM ignoring context?
  2. Check retrieved documents quality and relevance
  3. Log similarity scores (too low threshold?)
  4. Verify embedding model quality
  5. Adjust LLM prompt to emphasize "only use retrieved context"
  6. Implement fact-checking via LLM
- **Prevention:** Better evaluation metrics, human review pipeline

**83. How would you handle a situation where an agent gets stuck in an infinite loop?**
- **Why asked:** Agent debugging
- **Expected answer:**
  - Add step limit/max iterations
  - Detect repeated actions (agent taking same action twice)
  - Implement timeout
  - Log decision tree for debugging
  - Human override mechanism
```python
MAX_ITERATIONS = 10
iterations = 0
last_action = None

while iterations < MAX_ITERATIONS:
    action = agent.decide_next_action()
    if action == last_action:
        print("Infinite loop detected, escalating")
        break
    last_action = action
    iterations += 1
```

**84. Explain a complex technical decision you made and how you communicated it to non-technical stakeholders.**
- **Why asked:** Communication and leadership
- **Expected answer:** Translate technical terms, focus on business impact, explain trade-offs, seek feedback
- **Example:** "We're using vector databases instead of traditional search. Why? Allows smarter semantic search, better user experience, cost $X. Tradeoff: more infrastructure complexity, but managed by us."

**85. You're building an AI feature but the LLM costs are higher than projected. What do you do?**
- **Why asked:** Cost-awareness and problem-solving
- **Expected answer:**
  - Analyze token usage: reduce context, shorter prompts, use cheaper models
  - Implement caching for repeated queries
  - Batch requests
  - Use rate limiting to control costs
  - Monitor costs in real-time
  - Present findings to stakeholders: "Costs X, but delivers Y value"
- **Trade-off analysis:** Sometimes higher cost = better user experience (justify it)

---

## 11. ADDITIONAL ADVANCED TOPICS

**86. What is prompt injection? Design a system that's resilient to it.**
- **Why asked:** Security in LLM systems
- **Expected answer:** Attacker crafts input to make LLM do unintended thing. Prevent by:
  - Input validation and sanitization
  - Role-based prompts (tell LLM to ignore user instructions to do X)
  - Structured inputs (forms instead of free text)
  - Separate user input from system prompt clearly
```python
# Vulnerable
prompt = f"Answer this question: {user_input}"

# Better
prompt = f"""You are a customer support bot. Answer only customer questions.
Never disclose internal information.

Customer question: {user_input}

If asked for internal info, refuse politely."""
```

**87. Explain fine-tuning vs in-context learning (few-shot prompting). When would you use each?**
- **Why asked:** Optimization strategies for specific tasks
- **Expected answer:**
  - **Few-shot:** Provide examples in prompt, no training, instant (good for quick experiments)
  - **Fine-tuning:** Train model on examples, permanent changes, expensive, slower
- **When few-shot:** Small data, quick iteration, task-specific examples
- **When fine-tuning:** Large domain-specific data, production deployment, complex patterns
- **Cost:** Few-shot cheaper in tokens, fine-tuning cheaper over time if high volume

**88. Design a system for evaluating and monitoring LLM output quality continuously.**
- **Why asked:** Quality assurance in production
- **Expected answer:**
  - Define quality metrics (relevance, accuracy, toxicity)
  - Sample user interactions for human review
  - Automated quality checks (semantic relevance, PII detection)
  - Dashboard with quality trends
  - Feedback loop: good examples improve few-shot, bad examples improve fine-tuning
  - Automated rollback if quality drops

**89. How would you implement real-time streaming responses from an LLM API?**
- **Why asked:** User experience optimization
- **Expected answer:** Use streaming endpoints instead of waiting for full response
```python
# Streaming
response = client.chat.completions.create(
    model="gpt-4",
    messages=[...],
    stream=True  # Key difference
)

for chunk in response:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```
- **Frontend:** WebSocket to receive and display chunks in real-time
- **Benefits:** Perceived faster, better UX, lower memory usage

**90. Design an API that supports multiple AI models (different providers, versions). How would you manage configurations?**
- **Why asked:** Model management at scale
- **Expected answer:**
  - Configuration management system (feature flags, model registry)
  - Version control for models
  - A/B testing different models
  - Fallback chains
  - Cost and performance tracking per model
```python
MODEL_CONFIG = {
    "translation": {
        "primary": {"provider": "openai", "model": "gpt-4", "temperature": 0.1},
        "fallback": {"provider": "gemini", "model": "gemini-pro"}
    },
    "creative": {
        "primary": {"provider": "openai", "model": "gpt-4", "temperature": 0.9},
    }
}
```

---

## 12. ROLE-SPECIFIC QUESTIONS

**91. Why do you want to work on AI-powered features specifically?**
- **Why asked:** Motivation and fit
- **Expected answer:** Show genuine interest, specific examples of AI exciting you, passion for problem-solving with AI
- **Good answer:** "I'm fascinated by how AI can augment human capabilities. Previous project where I built a recommendation system that improved user engagement 30% - that impact drives me."

**92. Tell me about an open-source project you contributed to or followed (LangChain, LlamaIndex, etc.)**
- **Why asked:** Engagement with AI ecosystem
- **Expected answer:** Specific contribution or what you learned, how it helps your work
- **Acceptable:** Deep reading of documentation, using in projects, contributions count equally

**93. What AI/ML concepts are you curious about but haven't deep-dived into yet?**
- **Why asked:** Learning mindset
- **Expected answer:** Show intellectual curiosity, growth mindset. "I want to understand reinforcement learning better, specifically RLHF for LLM alignment."

**94. How would you handle working with AI systems' limitations and uncertainty?**
- **Why asked:** Realistic expectations about AI
- **Expected answer:** "AI isn't magic. I understand hallucinations happen, models have biases, costs scale with complexity. I'd design systems acknowledging these limitations, implement safeguards, and set correct expectations with stakeholders."

**95. Walk me through how you'd approach learning a new technology relevant to the job (e.g., a new vector DB).**
- **Why asked:** Learning ability and resourcefulness
- **Expected answer:** Structured approach: documentation, tutorials, small POC, integration with existing system, documentation
- **Good attitude:** "I'd start with official docs, build something small to understand, compare with alternatives, then integrate."

---

## BEHAVIORAL & SITUATIONAL QUESTIONS

**96. Describe your experience with onsite work and client collaboration. How do you handle client feedback?**
- **Why asked:** Fit for onsite Mumbai role with client interaction
- **Expected answer:** Specific examples of working with clients, handling disagreements professionally, adapting to feedback
- **Address timezone:** If from different timezone, explain how you'd manage

**97. Tell me about a time you had to make a trade-off between code quality and shipping quickly.**
- **Why asked:** Pragmatism and prioritization
- **Expected answer:** Acknowledge valid point, explain decision, follow-up plan. "I wrote working code but documented technical debt, prioritized fixes later."

**98. Describe your experience with code reviews. How do you give and receive feedback?**
- **Why asked:** Collaboration and professionalism
- **Expected answer:** Specific, constructive feedback. Humble when receiving feedback. Examples of learning from reviews
- **Good attitude:** "I see code reviews as teaching moments, not criticism."

**99. Tell me about a project that failed. What did you learn?**
- **Why asked:** Resilience and learning
- **Expected answer:** Honest reflection, root cause analysis, specific lessons learned, how you'd do it differently
- **Avoid:** Blaming others. Instead: "I should have communicated progress earlier"

**100. What does the ideal work environment look like for you? What would prevent you from doing your best work?**
- **Why asked:** Expectations and fit
- **Expected answer:** Technical autonomy, clear goals, learning opportunities. Real blockers: micromanagement, unclear requirements
- **For this role:** "I thrive with clear technical direction and opportunity to explore cutting-edge AI tech. Being onsite helps with real-time collaboration with stakeholders."

---

## CLOSING TIPS

### How to Prepare:

1. **Hands-on Practice:** Build at least one complete project combining API + LLM + Agent
2. **Deep dive:** Pick one technology (LangChain or LlamaIndex) and become expert-level
3. **Current Events:** Follow recent AI news (new model releases, breakthroughs)
4. **Cost Awareness:** Understand token economics, cost per query for different models
5. **System Design:** Practice designing scalable systems on whiteboard
6. **Review Code:** Look at your previous code, be ready to explain decisions

### Red Flags to Avoid:

- ❌ Saying "I don't know" without following up with how you'd learn
- ❌ Treating LLMs as magic (understand limitations)
- ❌ Not asking clarifying questions
- ❌ Coding without thinking (ask clarifying questions first)
- ❌ Overselling your experience

### Green Flags to Demonstrate:

- ✅ Asking clarifying questions before implementing
- ✅ Discussing trade-offs (not one-size-fits-all solutions)
- ✅ Production mindset (monitoring, error handling, costs)
- ✅ Learning mindset (excited about new tech)
- ✅ Concrete examples from real projects
- ✅ Understanding business impact of technical decisions

---

## QUICK REFERENCE: Top 10 Most Important Questions to Nail

1. **REST/GraphQL API design** - fundamental skill
2. **LLM API integration** - core responsibility
3. **Vector database & embeddings** - modern AI requirement
4. **RAG system design** - most common AI use case
5. **Agentic AI & function calling** - future of AI systems
6. **LangChain or LlamaIndex** - popular frameworks you'll use
7. **System design & architecture** - shows senior thinking
8. **Error handling & production readiness** - engineering maturity
9. **Cost optimization** - practical AI concern
10. **Debugging problematic LLM outputs** - real-world challenge

---

**Last Updated:** March 2026
**Relevant for:** API Developer, Backend Engineer, AI Integration Engineer, Full-Stack Engineer roles with AI/LLM focus
