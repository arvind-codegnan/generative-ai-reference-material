# Generative AI Notes for Java Freshers

> **Scope:** Java 21 and Core Java concepts only. These notes do not use Spring or Spring Boot.  
> **Audience:** Java freshers who are new to Artificial Intelligence and Generative AI.

## Table of Contents

- [1. What Is Generative AI?](#1-what-is-generative-ai)
- [2. Why Should Java Developers Learn Generative AI?](#2-why-should-java-developers-learn-generative-ai)
- [3. AI vs Machine Learning vs Deep Learning vs Generative AI](#3-ai-vs-machine-learning-vs-deep-learning-vs-generative-ai)
- [4. Basic Generative AI Workflow](#4-basic-generative-ai-workflow)
- [5. Types of Generative AI Models](#5-types-of-generative-ai-models)
- [6. Large Language Models](#6-large-language-models)
- [7. Tokens and Tokenization](#7-tokens-and-tokenization)
- [8. Prompts](#8-prompts)
- [9. Message Roles](#9-message-roles)
- [10. Prompt Templates](#10-prompt-templates)
- [11. Prompt Engineering](#11-prompt-engineering)
- [12. Model Parameters](#12-model-parameters)
- [13. Context Windows](#13-context-windows)
- [14. Structured Output](#14-structured-output)
- [15. Embeddings](#15-embeddings)
- [16. Vector Databases](#16-vector-databases)
- [17. Retrieval-Augmented Generation](#17-retrieval-augmented-generation)
- [18. Tool Calling](#18-tool-calling)
- [19. Core Java Integration Architecture](#19-core-java-integration-architecture)
- [20. Calling an AI API with Java HttpClient](#20-calling-an-ai-api-with-java-httpclient)
- [21. Asynchronous and Streaming Responses](#21-asynchronous-and-streaming-responses)
- [22. Conversation Memory](#22-conversation-memory)
- [23. Evaluating AI Responses](#23-evaluating-ai-responses)
- [24. Security Privacy and Responsible AI](#24-security-privacy-and-responsible-ai)
- [25. Cost and Performance](#25-cost-and-performance)
- [26. Generative AI Best Practices](#26-generative-ai-best-practices)
- [27. Common Generative AI Errors](#27-common-generative-ai-errors)
- [28. Frequently Asked Interview Questions](#28-frequently-asked-interview-questions)
- [29. Quick Revision](#29-quick-revision)

## 1. What Is Generative AI?

**Generative AI** is a category of Artificial Intelligence that creates new content after learning patterns from existing data.

It can generate:

- Natural-language text
- Programming code
- Images
- Audio
- Video
- Summaries
- Questions and answers
- Structured data such as JSON

A traditional application follows rules explicitly written by a programmer. A Generative AI application sends instructions and context to a trained model, and the model produces a probabilistic response.

```text
Traditional Program

Input → Programmer-Written Rules → Predictable Output

Generative AI Application

Prompt + Context → Trained AI Model → Generated Output
```

The generated response is not guaranteed to be identical for every request. It must therefore be validated before being trusted or used by business logic.

## 2. Why Should Java Developers Learn Generative AI?

Java is widely used for enterprise systems, APIs, financial applications, e-commerce platforms, and backend services. These applications can use Generative AI to add features such as:

- Intelligent question answering
- Document summarization
- Content generation
- Code explanation
- Customer-support assistants
- Semantic search
- Document classification
- Data extraction
- Natural-language interfaces
- Automated report generation

Java developers do not usually train large models. Their primary responsibility is to integrate models safely with applications, data, APIs, and business rules.

Important Java responsibilities include:

- Creating and managing prompts
- Calling model APIs
- Converting JSON responses into Java objects
- Handling errors and timeouts
- Protecting credentials and private data
- Validating generated output
- Connecting models with enterprise data
- Monitoring cost, latency, and quality

## 3. AI vs Machine Learning vs Deep Learning vs Generative AI

| Term | Meaning | Example |
| --- | --- | --- |
| Artificial Intelligence | Broad field of building systems that perform intelligent tasks | Rule-based expert system |
| Machine Learning | Systems learn patterns from data instead of using only fixed rules | Spam classifier |
| Deep Learning | Machine learning using multi-layer neural networks | Image recognition model |
| Generative AI | Models generate new text, images, audio, code, or other content | Chat assistant |

The relationship can be understood as:

```text
Artificial Intelligence
    └── Machine Learning
          └── Deep Learning
                └── Generative AI Models
```

Generative AI is not a replacement for traditional programming. A real application normally combines deterministic Java code with probabilistic model output.

## 4. Basic Generative AI Workflow

A basic Generative AI request follows these steps:

1. The user submits a question or instruction.
2. The Java application validates the input.
3. The application constructs a prompt.
4. The application sends the prompt to an AI model.
5. The model generates a response.
6. The Java application validates and processes the response.
7. The final result is returned to the user.

```text
User
  |
  v
Java Application
  |
  | Prompt and Configuration
  v
AI Model API
  |
  | Generated Response
  v
Validation and Processing
  |
  v
User
```

The Java application remains responsible for authentication, validation, authorization, error handling, logging, and business decisions.

## 5. Types of Generative AI Models

Different models accept and produce different data types.

| Model type | Common input | Common output | Example use case |
| --- | --- | --- | --- |
| Text generation | Text | Text | Question answering |
| Code generation | Text or code | Code | Method generation |
| Image generation | Text | Image | Product illustration |
| Speech recognition | Audio | Text | Meeting transcription |
| Text-to-speech | Text | Audio | Voice assistant |
| Multimodal | Text, image, or audio | Text or media | Image-based question answering |
| Embedding | Text or media | Numeric vector | Semantic search |

A Java application should select a model according to the required input, output, quality, latency, privacy, and cost.

## 6. Large Language Models

A **Large Language Model**, commonly called an **LLM**, is trained on large collections of text and code. It learns statistical relationships between tokens and generates a response by predicting likely subsequent tokens.

LLMs can perform tasks such as:

- Answering questions
- Explaining concepts
- Summarizing documents
- Translating text
- Generating code
- Extracting information
- Classifying text
- Rewriting content

An LLM does not understand information in the same way that a human does. It calculates likely outputs from patterns learned during training and from context supplied in the current request.

### Important Characteristics

- **Pre-trained:** It learns general patterns before application use.
- **Probabilistic:** It may produce different outputs for similar requests.
- **Context-dependent:** Its response depends heavily on the supplied prompt.
- **Knowledge-limited:** Its built-in knowledge is limited by its training and configuration.
- **Fallible:** It can produce convincing but incorrect information.

## 7. Tokens and Tokenization

A **token** is a unit of text processed by a language model. A token may represent:

- A complete word
- Part of a word
- Punctuation
- A number
- A symbol
- Whitespace combined with text

The conversion of text into tokens is called **tokenization**.

```text
Input Text
    |
    v
Tokenizer
    |
    v
Token IDs
    |
    v
Language Model
```

Token counts are important because they affect:

- Request cost
- Response cost
- Context-window usage
- Response time
- Maximum input and output size

Tokenization differs between models. Java applications should use provider-supplied token-counting tools when an exact count is required.

## 8. Prompts

A **prompt** is the instruction and context sent to a Generative AI model.

Simple prompt:

```text
Explain encapsulation in Java.
```

Improved prompt:

```text
Explain encapsulation to a Java fresher.
Use simple language, one real-world analogy,
and one Java 21 code snippet.
Limit the answer to 250 words.
```

An effective prompt normally specifies:

- The task
- The target audience
- Relevant context
- Constraints
- Expected output format
- Examples when necessary

Prompt quality strongly influences response quality.

## 9. Message Roles

Chat-based model APIs often organize prompts as messages with roles.

| Role | Purpose |
| --- | --- |
| System | Defines overall behavior, rules, and boundaries |
| User | Contains the user's request |
| Assistant | Represents earlier model responses |
| Tool | Contains the result returned by an application tool |

Example conversation structure:

```text
System: You are a Java trainer. Use beginner-friendly language.
User: Explain method overloading.
Assistant: Method overloading means...
User: Give one more example.
```

Applications should keep trusted system instructions separate from untrusted user input whenever the API supports roles.

## 10. Prompt Templates

A prompt template contains reusable text with placeholders for runtime values.

```text
Explain {topic} to a {audience}.
Include {exampleCount} examples.
```

Java can build a basic template with `String.formatted()`:

```java
String template = """
        Explain %s to %s.
        Include %d examples.
        """;

String prompt = template.formatted(
        "interfaces",
        "a Java fresher",
        2
);

System.out.println(prompt);
```

Do not insert untrusted text into a prompt without validation. User data can contain instructions that attempt to override application rules.

## 11. Prompt Engineering

**Prompt engineering** is the process of designing, testing, and improving prompts to obtain useful responses.

### Common Techniques

| Technique | Description |
| --- | --- |
| Clear instruction | State exactly what the model must do |
| Role prompting | Assign the model an appropriate role |
| Context provision | Supply information required to answer |
| Output constraints | Specify length, tone, structure, or format |
| Few-shot prompting | Include examples of desired input and output |
| Decomposition | Divide a complex task into smaller steps |
| Grounding | Require the answer to use supplied evidence |

### Example

Weak prompt:

```text
Tell me about Java.
```

Better prompt:

```text
Create revision notes on Java interfaces for a fresher.
Include a definition, syntax, one practical example,
three differences from abstract classes, and five interview questions.
Do not exceed 700 words.
```

Prompts should be tested against realistic, incorrect, incomplete, and malicious inputs.

## 12. Model Parameters

Model APIs normally provide configuration parameters that influence generation.

| Parameter | General purpose |
| --- | --- |
| Model | Selects the model used for the request |
| Temperature | Controls randomness or creativity |
| Maximum output tokens | Limits response length |
| Stop sequence | Stops generation when a sequence appears |
| Top-p | Controls token selection using probability mass |
| Seed | May improve repeatability when supported |

### Temperature

A lower temperature generally produces more focused and predictable output. A higher temperature generally produces more varied output.

| Use case | Typical preference |
| --- | --- |
| Data extraction | Lower randomness |
| Classification | Lower randomness |
| Technical explanation | Low to moderate randomness |
| Creative writing | Moderate to higher randomness |

Exact parameter names, ranges, behavior, and availability differ between providers and models.

## 13. Context Windows

The **context window** is the maximum amount of tokenized information a model can process during one request.

The context may include:

- System instructions
- User messages
- Conversation history
- Retrieved documents
- Tool definitions
- Tool results
- Generated output allowance

```text
Context Window

[System Instructions]
[Conversation History]
[User Question]
[Retrieved Information]
[Space for Generated Answer]
```

If the combined information is too large, the application must reduce it by:

- Removing irrelevant history
- Summarizing older messages
- Splitting documents into chunks
- Retrieving only relevant chunks
- Limiting tool output
- Restricting response length

Applications must reserve sufficient space for the generated response instead of filling the complete context window with input.

## 14. Structured Output

AI models usually return text. Even when the text resembles JSON, it must still be parsed and validated before it becomes a trusted Java object.

Example desired response:

```json
{
  "topic": "Encapsulation",
  "difficulty": "Beginner",
  "keyPoints": [
    "Protects object state",
    "Uses access modifiers",
    "Provides controlled access"
  ]
}
```

A corresponding Java record could be:

```java
import java.util.List;

public record TopicSummary(
        String topic,
        String difficulty,
        List<String> keyPoints
) {
}
```

The typical flow is:

```text
Prompt with Output Schema
          |
          v
       AI Model
          |
          v
      JSON Text
          |
          v
JSON Parsing and Validation
          |
          v
      Java Record
```

Use a JSON library to serialize and deserialize dynamic data. Do not build complex JSON through string concatenation, and do not trust fields until they pass application validation.

## 15. Embeddings

An **embedding** is a numeric vector that represents the meaning of text, an image, audio, or another content type.

Simplified example:

```text
"Java interface" → [0.18, -0.42, 0.71, ...]
"Java abstraction" → [0.21, -0.39, 0.69, ...]
```

Semantically related content generally produces vectors that are closer together.

Embeddings are commonly used for:

- Semantic search
- Document similarity
- Recommendations
- Classification
- Clustering
- Retrieval-Augmented Generation

The number of values in an embedding vector is called its **dimensionality**.

## 16. Vector Databases

A **vector database** or **vector store** stores embeddings and searches for similar vectors.

A stored item normally includes:

- A unique identifier
- Original text or a reference to it
- Embedding vector
- Metadata such as source, topic, or date

```text
Document Chunk
      |
      v
Embedding Model
      |
      v
Numeric Vector + Metadata
      |
      v
Vector Database
```

When a user submits a question, the application creates an embedding for the question and searches for nearby vectors.

Vector similarity is commonly calculated using measures such as cosine similarity, dot product, or Euclidean distance. The selected database and embedding model determine the supported approach.

## 17. Retrieval-Augmented Generation

**Retrieval-Augmented Generation**, or **RAG**, provides a model with relevant external information before it generates an answer.

RAG is useful when the model must answer using:

- Organization documents
- Product manuals
- Policies
- Training notes
- Frequently asked questions
- Current application data

### Indexing Phase

1. Read documents.
2. Clean the content.
3. Split content into meaningful chunks.
4. Generate an embedding for every chunk.
5. Store vectors, content, and metadata.

### Question-Answering Phase

1. Receive the user's question.
2. Generate an embedding for the question.
3. Retrieve similar document chunks.
4. Add those chunks to the prompt.
5. Ask the model to answer from the supplied context.
6. Return the answer with source references when possible.

```text
Documents → Split → Embed → Vector Store
                              |
User Question → Embed → Similarity Search
                              |
                              v
                 Retrieved Context + Question
                              |
                              v
                          AI Model
                              |
                              v
                       Grounded Answer
```

RAG does not guarantee correctness. Retrieval quality, chunking, prompt design, source quality, and response validation all affect the result.

## 18. Tool Calling

**Tool calling** allows a model to request that the Java application execute an approved function.

Possible tools include:

- Check an order status
- Retrieve a customer record
- Calculate tax
- Search a product catalogue
- Get current inventory
- Schedule an appointment

The model does not execute Java methods directly. It returns a structured request, and the application decides whether to execute it.

```text
Java Application → Prompt + Tool Definitions → AI Model
Java Application ← Tool Name + Arguments   ← AI Model
Java Application → Validate and Execute Tool
Java Application → Tool Result              → AI Model
Java Application ← Final Response            ← AI Model
```

### Example Tool Contract

```java
public interface OrderStatusTool {

    String findStatus(long orderId);
}
```

Tool execution must use allowlisted functions, validated arguments, authorization checks, timeouts, audit logging, and controlled side effects.

## 19. Core Java Integration Architecture

A Spring-free Java application can organize Generative AI integration into simple layers.

```text
Console or Web Client
        |
        v
Application Service
        |
        v
Prompt Builder
        |
        v
AI Client Interface
        |
        v
Java HttpClient
        |
        v
External AI API
```

### Suggested Responsibilities

| Component | Responsibility |
| --- | --- |
| Input layer | Reads and validates user input |
| Application service | Coordinates the use case |
| Prompt builder | Creates prompts from templates and context |
| AI client | Hides provider-specific HTTP details |
| JSON mapper | Converts Java objects and JSON |
| Validator | Checks generated output |
| Repository | Reads local or database-backed knowledge |

### Provider-Neutral Interface

```java
import java.io.IOException;

public interface GenerativeModel {

    String generate(String prompt)
            throws IOException, InterruptedException;
}
```

Keeping an interface between business logic and the external provider makes testing and provider replacement easier.

## 20. Calling an AI API with Java HttpClient

Java 21 includes `java.net.http.HttpClient`, which can send HTTP requests without Spring.

The following example uses a placeholder endpoint and model name. Replace them according to the selected provider's documentation.

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

public class BasicAiRequestExample {

    public static void main(String[] args) throws Exception {

        String apiKey = System.getenv("AI_API_KEY");

        if (apiKey == null || apiKey.isBlank()) {
            throw new IllegalStateException(
                    "AI_API_KEY environment variable is missing");
        }

        String requestBody = """
                {
                  "model": "example-model",
                  "input": "Explain inheritance to a Java fresher."
                }
                """;

        HttpClient client = HttpClient.newBuilder()
                .connectTimeout(Duration.ofSeconds(10))
                .build();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(
                        "https://api.example.ai/v1/generate"))
                .timeout(Duration.ofSeconds(30))
                .header("Authorization", "Bearer " + apiKey)
                .header("Content-Type", "application/json")
                .POST(HttpRequest.BodyPublishers
                        .ofString(requestBody))
                .build();

        HttpResponse<String> response = client.send(
                request,
                HttpResponse.BodyHandlers.ofString()
        );

        if (response.statusCode() >= 200
                && response.statusCode() < 300) {
            System.out.println(response.body());
        } else {
            System.err.println(
                    "AI request failed with status: "
                            + response.statusCode());
        }
    }
}
```

### Important Points

- Read API keys from protected runtime configuration.
- Use an actual JSON library for dynamic request bodies.
- Set connection and request timeouts.
- Check the HTTP status before processing the body.
- Parse successful responses into validated Java objects.
- Do not log API keys or sensitive prompts.
- Treat provider error messages as untrusted external data.

## 21. Asynchronous and Streaming Responses

An asynchronous request allows the Java thread to continue other work while waiting for the model API.

```java
import java.net.http.HttpResponse;
import java.util.concurrent.CompletableFuture;

CompletableFuture<String> responseFuture = client
        .sendAsync(
                request,
                HttpResponse.BodyHandlers.ofString()
        )
        .thenApply(response -> {
            if (response.statusCode() < 200
                    || response.statusCode() >= 300) {
                throw new IllegalStateException(
                        "Request failed: "
                                + response.statusCode());
            }

            return response.body();
        });

responseFuture
        .thenAccept(System.out::println)
        .exceptionally(error -> {
            System.err.println(error.getMessage());
            return null;
        });
```

### Streaming

Streaming delivers pieces of the generated response as they become available instead of waiting for the entire response.

```text
Non-Streaming

Request → Wait for Complete Generation → Full Response

Streaming

Request → Chunk 1 → Chunk 2 → Chunk 3 → Completion
```

Streaming protocols and event formats differ by provider. A Java client may need to process server-sent events, line-delimited JSON, or another streaming format.

## 22. Conversation Memory

Models do not automatically remember earlier application conversations. The Java application must resend the required history or a summary of it.

Example message record:

```java
public record ChatMessage(
        String role,
        String content
) {
}
```

Example in-memory conversation:

```java
import java.util.ArrayList;
import java.util.List;

List<ChatMessage> history = new ArrayList<>();

history.add(new ChatMessage(
        "user",
        "Explain polymorphism."
));

history.add(new ChatMessage(
        "assistant",
        "Polymorphism allows one interface..."
));
```

### Memory Strategies

| Strategy | Description |
| --- | --- |
| Full history | Sends every earlier message |
| Sliding window | Sends only the most recent messages |
| Summary memory | Replaces older messages with a summary |
| Persistent memory | Stores selected history in a database |

Conversation memory must be limited because it consumes context-window space and may contain personal or sensitive data.

## 23. Evaluating AI Responses

AI responses must be evaluated for quality and safety.

Common evaluation criteria include:

- Relevance
- Correctness
- Completeness
- Consistency with supplied context
- Readability
- Required format compliance
- Safety
- Absence of unsupported claims

### Evaluation Approaches

| Approach | Description |
| --- | --- |
| Rule-based checks | Validates length, required fields, patterns, or values |
| Reference comparison | Compares the response with an expected answer |
| Human review | A person assesses quality |
| Model-based evaluation | Another model scores the response using criteria |
| Grounding check | Verifies whether claims are supported by supplied sources |

Generative AI tests should not always expect one exact string. They should test required facts, structure, boundaries, safety, and acceptable variation.

## 24. Security Privacy and Responsible AI

Generative AI applications introduce risks beyond ordinary HTTP integration.

### Major Risks

| Risk | Description |
| --- | --- |
| Prompt injection | Untrusted text attempts to override application instructions |
| Data leakage | Private information is sent or exposed unintentionally |
| Hallucination | The model produces unsupported or incorrect content |
| Unsafe tool use | A model requests a harmful or unauthorized action |
| Insecure output handling | Generated content is executed or rendered without validation |
| Excessive permissions | Tools or database accounts have more access than required |
| Bias | Output reflects unfair patterns from data or model behavior |

### Security Guidelines

- Never place API keys in source code.
- Keep trusted instructions separate from user content.
- Validate input length and allowed content.
- Minimize personal and confidential data sent to a model.
- Validate and sanitize generated output.
- Never execute generated code automatically.
- Require authorization before tool execution.
- Allowlist tools and validate every argument.
- Record security-relevant actions in audit logs.
- Apply rate limits and request-size limits.
- Provide human review for high-impact decisions.

Generated output must never be treated as automatically correct, safe, or authorized.

## 25. Cost and Performance

Hosted AI usage commonly depends on input tokens, output tokens, selected model, and additional features.

Cost and latency can be reduced by:

- Selecting an appropriate model for the task
- Limiting unnecessary prompt text
- Retrieving only relevant document chunks
- Restricting maximum output size
- Summarizing long conversation history
- Caching safe and reusable results
- Using batch processing when supported
- Applying timeouts and concurrency limits
- Avoiding repeated embedding generation for unchanged data

### Performance Measurements

| Measurement | Meaning |
| --- | --- |
| Request latency | Total time required for a response |
| Time to first token | Delay before streaming output begins |
| Input tokens | Tokens sent to the model |
| Output tokens | Tokens generated by the model |
| Error rate | Percentage of failed requests |
| Retrieval latency | Time required to locate RAG context |
| Evaluation score | Measured response quality |

An application should balance quality, speed, privacy, and cost instead of automatically selecting the largest model.

## 26. Generative AI Best Practices

- Define the business problem before selecting a model.
- Keep provider-specific code behind a Java interface.
- Use Java records or DTOs for request and response data.
- Use a JSON library for serialization and deserialization.
- Store API keys outside source code.
- Set connection and request timeouts.
- Implement controlled retry with backoff for retryable failures.
- Validate HTTP status codes and response bodies.
- Limit prompt and response sizes.
- Validate structured output against application rules.
- Ground factual answers with trusted information.
- Include source references in RAG responses when possible.
- Test prompts with normal, incorrect, incomplete, and malicious inputs.
- Log request identifiers, latency, token usage, and error categories.
- Avoid logging secrets and sensitive prompt content.
- Require authorization and validation before tool execution.
- Keep humans involved in high-impact decisions.
- Monitor quality after deployment.

## 27. Common Generative AI Errors

| Problem | Possible reason | Suggested action |
| --- | --- | --- |
| `401 Unauthorized` | Missing or invalid API key | Verify protected runtime configuration |
| `403 Forbidden` | Account or model access is not allowed | Check account permissions |
| `404 Not Found` | Incorrect endpoint or model name | Check provider documentation |
| `429 Too Many Requests` | Rate or quota limit exceeded | Retry with backoff and respect retry information |
| Request timeout | Model or network response is slow | Use realistic timeouts and cancellation |
| Context-length error | Prompt exceeds the model limit | Reduce history or retrieved content |
| Invalid JSON response | Output does not match the required format | Validate, repair cautiously, or retry with constraints |
| Empty response | Filtering, model behavior, or parsing problem | Inspect status and provider metadata |
| Irrelevant response | Weak prompt or poor retrieved context | Improve instructions and retrieval |
| Hallucinated facts | Model lacks reliable grounding | Supply trusted context and verify claims |
| High cost | Excessive tokens or unsuitable model | Add budgets and optimize prompts |
| Leaked secret | Credential included in code or logs | Revoke it immediately and correct secret handling |

Do not retry every failure. Authentication errors, validation failures, and permanent permission errors normally require correction rather than repeated requests.

## 28. Frequently Asked Interview Questions

### What Is Generative AI?

Generative AI is a category of AI that creates new content such as text, code, images, audio, or structured data after learning patterns from existing data.

### What Is an LLM?

An LLM is a Large Language Model trained on large collections of text and code. It generates responses by predicting likely token sequences from the supplied context.

### What Is a Prompt?

A prompt is the instruction and context sent to a Generative AI model.

### What Is a Token?

A token is a unit of text processed by a language model. It may be a word, word fragment, symbol, or punctuation.

### What Is a Context Window?

The context window is the maximum tokenized information a model can process in one request, including input, history, context, tool information, and output allowance.

### What Is Temperature?

Temperature is a model parameter that generally controls randomness. Lower values usually produce more focused output, while higher values usually produce more varied output.

### What Is an Embedding?

An embedding is a numeric vector representing the semantic meaning of content.

### What Is a Vector Database?

A vector database stores embeddings and supports similarity searches to find semantically related content.

### What Is RAG?

Retrieval-Augmented Generation retrieves relevant external information and adds it to the model prompt before the answer is generated.

### What Is Tool Calling?

Tool calling allows a model to request that an application execute an approved function. The application validates and performs the action.

### What Is a Hallucination?

A hallucination is a fluent but unsupported, misleading, or incorrect model response.

### How Can Java Call an AI API Without Spring?

Java can use `java.net.http.HttpClient` to send HTTPS requests, along with a JSON library to serialize request objects and parse response objects.

### Why Should Generated JSON Be Validated?

The model returns probabilistic text. Even JSON-looking output can be malformed, incomplete, or contain invalid business values.

### Does an AI Model Remember Earlier Messages Automatically?

Usually not at the application level. The application must send the required conversation history, summary, or stored memory with the request.

### How Should API Keys Be Stored?

API keys should be stored in protected environment variables or a secret-management system, never hard-coded in source code.

### Should Generated Code Be Executed Automatically?

No. Generated code must be reviewed, validated, isolated, and controlled before execution.

### How Do You Test a Generative AI Application?

Test required facts, structure, grounding, safety, latency, cost, and acceptable variation. Include normal, edge-case, and malicious inputs.

### Can Generative AI Replace Java Developers?

Generative AI can assist with coding and analysis, but Java developers are still responsible for architecture, correctness, security, integration, testing, and business decisions.

## 29. Quick Revision

```text
Generative AI
 ├── Models
 │    ├── Large Language Models
 │    ├── Image and Audio Models
 │    └── Embedding Models
 ├── Prompting
 │    ├── Instructions
 │    ├── Context
 │    ├── Message Roles
 │    └── Output Format
 ├── Enterprise Integration
 │    ├── Structured Output
 │    ├── RAG
 │    ├── Tool Calling
 │    └── Conversation Memory
 ├── Core Java
 │    ├── HttpClient
 │    ├── CompletableFuture
 │    ├── Records and DTOs
 │    └── JSON Processing
 └── Production Concerns
      ├── Security and Privacy
      ├── Evaluation
      ├── Cost and Performance
      └── Monitoring
```

The most important Generative AI application flow is:

```text
Validate Input → Build Prompt → Call Model → Check Response
               → Parse Output → Validate Result → Return Safely
```

For RAG applications:

```text
Prepare Documents → Split → Embed → Store
User Question → Retrieve Relevant Context → Generate → Verify
```

For tool-calling applications:

```text
Define Allowed Tools → Model Requests Tool → Validate Arguments
                     → Authorize → Execute → Return Result → Generate Answer
```
