# Generative AI for Java Professionals

> # Part 2: Generative AI with Spring ##

## Table of Contents

- [1. What Does Spring Add to a Generative AI Application?](#1-what-does-spring-add-to-a-generative-ai-application)
- [2. Spring Boot and Spring AI](#2-spring-boot-and-spring-ai)
- [3. Core Java Integration vs Spring AI](#3-core-java-integration-vs-spring-ai)
- [4. Spring AI Application Architecture](#4-spring-ai-application-architecture)
- [5. Create the Maven Project](#5-create-the-maven-project)
- [6. Configure the Model Provider](#6-configure-the-model-provider)
- [7. Auto-Configuration and Dependency Injection](#7-auto-configuration-and-dependency-injection)
- [8. ChatModel and ChatClient](#8-chatmodel-and-chatclient)
- [9. Create the First AI Service](#9-create-the-first-ai-service)
- [10. Expose a REST Endpoint](#10-expose-a-rest-endpoint)
- [11. System Messages and Prompt Templates](#11-system-messages-and-prompt-templates)
- [12. Model Options](#12-model-options)
- [13. ChatResponse and Metadata](#13-chatresponse-and-metadata)
- [14. Structured Output into Java Records](#14-structured-output-into-java-records)
- [15. Streaming Responses](#15-streaming-responses)
- [16. Advisors](#16-advisors)
- [17. Conversation Memory](#17-conversation-memory)
- [18. Persistent Chat Memory with JDBC](#18-persistent-chat-memory-with-jdbc)
- [19. Tool Calling with Spring Methods](#19-tool-calling-with-spring-methods)
- [20. Embeddings with EmbeddingModel](#20-embeddings-with-embeddingmodel)
- [21. Documents and VectorStore](#21-documents-and-vectorstore)
- [22. ETL for AI Documents](#22-etl-for-ai-documents)
- [23. Retrieval-Augmented Generation](#23-retrieval-augmented-generation)
- [24. PGvector for a Production-Style RAG Application](#24-pgvector-for-a-production-style-rag-application)
- [25. Exception Handling and Resilience](#25-exception-handling-and-resilience)
- [26. Validation and API Design](#26-validation-and-api-design)
- [27. Observability and Logging](#27-observability-and-logging)
- [28. Testing and Evaluation](#28-testing-and-evaluation)
- [29. Security and Responsible AI](#29-security-and-responsible-ai)
- [30. Cost and Performance](#30-cost-and-performance)
- [31. Model Context Protocol](#31-model-context-protocol)
- [32. Suggested Package Structure](#32-suggested-package-structure)
- [33. Mini-Project: Java Learning Assistant](#33-mini-project-java-learning-assistant)
- [34. Common Errors](#34-common-errors)
- [35. Frequently Asked Interview Questions](#35-frequently-asked-interview-questions)

## 1. What Does Spring Add to a Generative AI Application?

In the earlier Core Java notes, an AI request required manual work such as:

- Creating an HTTP request
- Adding authentication headers
- Building JSON
- Sending the request
- Parsing the JSON response
- Handling provider-specific response formats
- Writing retry, configuration, and integration code

Spring AI provides higher-level Java abstractions for these tasks. Spring Boot adds configuration, dependency injection, web support, health checks, and production features.

```text
Without Spring AI

Java code → HTTP request → Provider-specific JSON → AI provider

With Spring AI

Java code → ChatClient / ChatModel → Spring AI integration → AI provider
```

Spring AI does not contain an LLM. It connects a Spring application to external or local AI models.

## 2. Spring Boot and Spring AI

Spring Boot and Spring AI solve different problems.

| Technology | Main responsibility |
| --- | --- |
| Spring Framework | Dependency injection, web MVC, validation, configuration, and application structure |
| Spring Boot | Auto-configuration, starters, embedded server, Actuator, and simplified application startup |
| Spring AI | Portable APIs for chat models, embeddings, vector stores, tools, memory, RAG, and related AI features |
| AI provider | Hosts or runs the actual model |

Typical startup flow:

```text
Spring Boot starts
      |
      v
Reads application.yml and environment variables
      |
      v
Spring AI auto-configures model clients
      |
      v
Your services receive ChatClient, ChatModel, or EmbeddingModel beans
```

The Spring AI API reduces provider-specific code, but model capabilities still differ. A feature supported by one provider or model may not be supported by another.

## 3. Core Java Integration vs Spring AI

| Core Java approach | Spring AI approach |
| --- | --- |
| `HttpClient` sends the request | `ChatClient` or `ChatModel` sends the request |
| Manually create provider JSON | Spring AI creates the provider request |
| Manually parse response JSON | `.content()` returns text and `.entity()` maps to a Java type |
| Write provider-specific DTOs | Use portable Spring AI request and response abstractions |
| Manually manage earlier messages | Use `ChatMemory` and a memory advisor |
| Manually implement RAG orchestration | Use `VectorStore` and RAG advisors |
| Manually describe and dispatch functions | Use `@Tool` methods or `ToolCallback` objects |
| Custom metrics around every call | Use Spring AI observations with Actuator and Micrometer |

Spring AI is an abstraction, not magic. The developer must still:

- Design good prompts
- Validate inputs and outputs
- Protect credentials
- Choose models deliberately
- Handle failures
- Control which tools the model may call
- Evaluate answer quality
- Monitor latency, tokens, and cost

## 4. Spring AI Application Architecture

A clean beginner-friendly structure separates HTTP, AI orchestration, business logic, and data access.

```text
Client
  |
  v
REST Controller
  |
  v
AI Application Service
  |
  +----------------------+----------------------+
  |                      |                      |
  v                      v                      v
ChatClient           VectorStore          Java Tools
  |                      |                      |
  v                      v                      v
Chat Model          Embedding Model       Business Services
```

Recommended responsibilities:

| Layer | Responsibility |
| --- | --- |
| Controller | HTTP input, validation, status codes, response DTOs |
| AI service | Prompt construction, model call, advisors, output validation |
| Business service | Deterministic application rules and database operations |
| Repository | Data persistence |
| Tool class | Carefully controlled operations exposed to the model |
| Configuration | Beans, model defaults, advisors, and security-related settings |

Do not place every operation in the controller. Thin controllers and focused services are easier to test and maintain.

## 5. Create the Maven Project

The following `pom.xml` provides the base application. It uses the Spring Boot parent and imports the Spring AI BOM.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.16</version>
        <relativePath/>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>java-ai-assistant</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>java-ai-assistant</name>

    <properties>
        <java.version>21</java.version>
        <spring-ai.version>1.1.8</spring-ai.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.ai</groupId>
                <artifactId>spring-ai-bom</artifactId>
                <version>${spring-ai.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-starter-model-openai</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

The Spring AI BOM manages compatible versions of Spring AI modules. Dependencies covered by the BOM should normally omit their individual versions.

### Main Class

```java
package com.example.javaai;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class JavaAiApplication {

    public static void main(String[] args) {
        SpringApplication.run(JavaAiApplication.class, args);
    }
}
```

Run the application with:

```bash
./mvnw spring-boot:run
```

## 6. Configure the Model Provider

Create `src/main/resources/application.yml`:

```yaml
spring:
  application:
    name: java-ai-assistant
  ai:
    model:
      chat: openai
    openai:
      api-key: ${OPENAI_API_KEY}
      chat:
        options:
          model: gpt-4o-mini
          temperature: 0.2

management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics
```

Set the secret outside the source code:

```bash
export OPENAI_API_KEY="your-key"
```

Never commit the real key to Git.

Important properties:

| Property | Purpose |
| --- | --- |
| `spring.ai.model.chat` | Selects the chat provider when multiple provider integrations are present |
| `spring.ai.openai.api-key` | Supplies the provider credential |
| `spring.ai.openai.base-url` | Overrides the provider base URL when needed |
| `spring.ai.openai.chat.options.model` | Sets the default chat model |
| `spring.ai.openai.chat.options.temperature` | Sets the default response randomness |

The model name is a provider value and can change independently of Spring AI. Select a model that is available to your account and supports the features you use.

## 7. Auto-Configuration and Dependency Injection

Because the OpenAI starter is present and configuration is supplied, Spring Boot creates useful beans such as:

- An OpenAI chat model implementation
- A `ChatModel` bean
- A prototype `ChatClient.Builder` bean
- An embedding model when the provider integration and configuration support it
- Supporting HTTP clients and observation components

Constructor injection is the recommended way to receive dependencies.

```java
@Service
public class ExplanationService {

    private final ChatClient chatClient;

    public ExplanationService(ChatClient.Builder builder) {
        this.chatClient = builder.build();
    }
}
```

Advantages of constructor injection:

- Required dependencies are explicit.
- Fields can be `final`.
- Unit tests can supply test doubles.
- The object cannot be created in an incomplete state.
- No field reflection is required.

`ChatClient.Builder` is a builder. `ChatClient` is the configured client created from it. Build the client once and reuse it unless requests require meaningfully different default configurations.

## 8. ChatModel and ChatClient

Spring AI offers both a lower-level model API and a higher-level fluent client.

| API | Use it when |
| --- | --- |
| `ChatModel` | You need direct access to `Prompt`, `ChatResponse`, or model-level behavior |
| `ChatClient` | You want a fluent API for system/user messages, templates, advisors, tools, streaming, and typed output |

### ChatModel Example

```java
@Service
public class DirectModelService {

    private final ChatModel chatModel;

    public DirectModelService(ChatModel chatModel) {
        this.chatModel = chatModel;
    }

    public String ask(String question) {
        return chatModel.call(question);
    }
}
```

### ChatClient Example

```java
@Service
public class FluentChatService {

    private final ChatClient chatClient;

    public FluentChatService(ChatClient.Builder builder) {
        this.chatClient = builder.build();
    }

    public String ask(String question) {
        return chatClient.prompt()
                .user(question)
                .call()
                .content();
    }
}
```

For most application code, begin with `ChatClient`. Learn `ChatModel` as well because `ChatClient` is built on model abstractions.

## 9. Create the First AI Service

A service can set stable behavior with a default system message.

```java
package com.example.javaai.chat;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.stereotype.Service;

@Service
public class JavaTutorService {

    private final ChatClient chatClient;

    public JavaTutorService(ChatClient.Builder builder) {
        this.chatClient = builder
                .defaultSystem("""
                        You are a patient Java tutor.
                        Teach Java 21 concepts to freshers.
                        Use simple language and short examples.
                        State uncertainty instead of inventing facts.
                        """)
                .build();
    }

    public String explain(String question) {
        return chatClient.prompt()
                .user(question)
                .call()
                .content();
    }
}
```

Flow:

```text
explain(question)
      |
      v
Default system message + user question
      |
      v
ChatClient → ChatModel → Provider
      |
      v
Generated text returned by content()
```

The model output is untrusted data. Do not treat it as an authorization decision, database command, or verified fact without application-level checks.

## 10. Expose a REST Endpoint

Use Java records for small request and response DTOs.

```java
package com.example.javaai.chat;

import jakarta.validation.constraints.NotBlank;

public record AskRequest(
        @NotBlank(message = "Question is required")
        String question) {
}
```

```java
package com.example.javaai.chat;

public record AskResponse(String answer) {
}
```

```java
package com.example.javaai.chat;

import jakarta.validation.Valid;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/ai")
public class JavaTutorController {

    private final JavaTutorService tutorService;

    public JavaTutorController(JavaTutorService tutorService) {
        this.tutorService = tutorService;
    }

    @PostMapping("/ask")
    public ResponseEntity<AskResponse> ask(
            @Valid @RequestBody AskRequest request) {

        String answer = tutorService.explain(request.question());
        return ResponseEntity.ok(new AskResponse(answer));
    }
}
```

Example request:

```http
POST /api/ai/ask
Content-Type: application/json

{
  "question": "Explain method overloading with one example."
}
```

Keep the controller responsible for HTTP concerns and the service responsible for AI orchestration.

## 11. System Messages and Prompt Templates

A system message defines stable behavior. A user message contains the current request.

```java
String answer = chatClient.prompt()
        .system("""
                You are a Java trainer.
                Use Java 21 syntax.
                Never invent a method that does not exist.
                """)
        .user("Explain the difference between List and Set.")
        .call()
        .content();
```

Use a template when part of the prompt changes at runtime.

```java
public String explainTopic(String topic, String level) {
    return chatClient.prompt()
            .user(user -> user
                    .text("""
                            Explain {topic} to a {level} Java learner.
                            Include a definition, one analogy, and one code example.
                            """)
                    .param("topic", topic)
                    .param("level", level))
            .call()
            .content();
}
```

Benefits of templates:

- Repeated instructions remain consistent.
- Variable values are separated from prompt wording.
- Prompts are easier to test and version.
- Prompt changes do not require string concatenation.

Prompt templates do not automatically prevent prompt injection. Validate input and keep permissions outside the model.

## 12. Model Options

Default options can be placed in configuration. Request-specific options can be supplied in Java.

```java
import org.springframework.ai.openai.OpenAiChatOptions;

public String createQuiz(String topic) {
    OpenAiChatOptions options = OpenAiChatOptions.builder()
            .model("gpt-4o-mini")
            .temperature(0.4)
            .build();

    return chatClient.prompt()
            .user("Create five beginner quiz questions about " + topic)
            .options(options)
            .call()
            .content();
}
```

Common options:

| Option | Effect |
| --- | --- |
| Model | Chooses a provider model |
| Temperature | Controls randomness; lower values are usually more focused |
| Maximum output tokens | Limits response length and cost |
| Stop sequences | Ask generation to stop at selected patterns |
| Top-p | Controls token sampling in a different way from temperature |

Use provider-neutral options where possible. Use provider-specific options only when the feature is worth the reduced portability.

Do not change temperature and top-p together unless you understand the combined effect.

## 13. ChatResponse and Metadata

`.content()` is convenient when only text is required. Use `.chatResponse()` when the application needs richer data.

```java
import org.springframework.ai.chat.model.ChatResponse;

ChatResponse response = chatClient.prompt()
        .user("Explain Java records in three sentences.")
        .call()
        .chatResponse();
```

A `ChatResponse` can contain:

- One or more generated results
- The generated assistant message
- Provider and model metadata
- Token-usage information when supplied by the provider
- Finish information

Possible uses:

- Record token usage for cost monitoring.
- Detect why generation stopped.
- Store the provider model name with an audit record.
- Inspect multiple generations when requested.

Metadata availability differs by provider. Code should not assume every provider returns every field.

## 14. Structured Output into Java Records

Business code should prefer typed data over parsing free-form text.

```java
package com.example.javaai.quiz;

import java.util.List;

public record JavaQuiz(
        String topic,
        List<Question> questions) {

    public record Question(
            String text,
            List<String> choices,
            int correctChoiceIndex,
            String explanation) {
    }
}
```

```java
@Service
public class QuizService {

    private final ChatClient chatClient;

    public QuizService(ChatClient.Builder builder) {
        this.chatClient = builder.build();
    }

    public JavaQuiz createQuiz(String topic) {
        return chatClient.prompt()
                .system("Create accurate quizzes for Java freshers.")
                .user("Create three multiple-choice questions about " + topic)
                .call()
                .entity(JavaQuiz.class);
    }
}
```

Conceptual flow:

```text
Java record
    |
    v
Schema and formatting instructions
    |
    v
AI model generates structured text
    |
    v
Spring converts text into the Java record
```

Structured conversion is still an AI-assisted operation. Validate the returned object:

- Check required fields.
- Check collection sizes.
- Check numeric ranges.
- Reject unexpected or unsafe values.
- Retry only when appropriate.
- Use provider-native structured output only with a model that supports it.

Never assume that successful deserialization proves factual correctness.

## 15. Streaming Responses

A normal call waits for the complete answer. Streaming returns smaller pieces as they are generated.

Add WebFlux support for streaming:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

Service method:

```java
import reactor.core.publisher.Flux;

public Flux<String> streamExplanation(String question) {
    return chatClient.prompt()
            .user(question)
            .stream()
            .content();
}
```

Server-Sent Events endpoint:

```java
import org.springframework.http.MediaType;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import reactor.core.publisher.Flux;

@GetMapping(value = "/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public Flux<String> stream(@RequestParam String question) {
    return tutorService.streamExplanation(question);
}
```

```text
Complete response: request ───────────────→ complete answer

Streaming response: request → chunk → chunk → chunk → complete
```

Streaming improves perceived responsiveness, but it adds concerns:

- The client must handle partial text.
- An error can occur after some text is already displayed.
- Moderation may need buffering.
- Backpressure and cancellation matter.
- Tool calling may introduce blocking behavior.

## 16. Advisors

An advisor intercepts and enriches a `ChatClient` request or response. Advisors are similar in spirit to filters or interceptors, but they are designed for AI interaction patterns.

Common uses:

- Conversation memory
- RAG context retrieval
- Request and response logging
- Safety checks
- Adding standard context
- Observability

```text
User prompt
    |
    v
Advisor 1 → Advisor 2 → Advisor 3
    |
    v
Chat model
    |
    v
Response passes back through the advisor chain
```

Logging example:

```java
import org.springframework.ai.chat.client.advisor.SimpleLoggerAdvisor;

String answer = chatClient.prompt()
        .advisors(new SimpleLoggerAdvisor())
        .user("Explain dependency injection.")
        .call()
        .content();
```

Enable its debug logger:

```yaml
logging:
  level:
    org.springframework.ai.chat.client.advisor: DEBUG
```

Do not enable prompt and response logging casually in production. Prompts can contain personal, confidential, or regulated information.

Advisor order matters. For example, memory may enrich a question before RAG performs retrieval.

## 17. Conversation Memory

LLMs are stateless. Spring AI memory stores selected earlier messages and supplies them to later calls.

Key abstractions:

| Type | Purpose |
| --- | --- |
| `ChatMemory` | Decides which messages are kept for model context |
| `ChatMemoryRepository` | Stores and retrieves messages |
| `MessageWindowChatMemory` | Keeps a bounded window of recent messages |
| `MessageChatMemoryAdvisor` | Connects memory to `ChatClient` calls |

Configuration:

```java
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.MessageChatMemoryAdvisor;
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.MessageWindowChatMemory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ChatConfiguration {

    @Bean
    ChatMemory chatMemory() {
        return MessageWindowChatMemory.builder()
                .maxMessages(20)
                .build();
    }

    @Bean
    ChatClient memoryChatClient(
            ChatClient.Builder builder,
            ChatMemory chatMemory) {

        return builder
                .defaultAdvisors(
                        MessageChatMemoryAdvisor.builder(chatMemory).build())
                .build();
    }
}
```

Every request using a memory advisor must include a conversation ID.

```java
public String chat(String conversationId, String message) {
    return chatClient.prompt()
            .user(message)
            .advisors(advisor -> advisor.param(
                    ChatMemory.CONVERSATION_ID,
                    conversationId))
            .call()
            .content();
}
```

```text
conversation-101 → messages for learner A
conversation-202 → messages for learner B
```

Never use one shared conversation ID for all users. That can leak context between users.

Chat memory is not the same as full chat history. Store the complete audit history separately when the application needs it.

## 18. Persistent Chat Memory with JDBC

In-memory storage disappears when the application restarts. Use a persistent repository for durable memory.

Add the JDBC memory starter and a database driver:

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-chat-memory-repository-jdbc</artifactId>
</dependency>

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

H2 demonstration configuration:

```yaml
spring:
  datasource:
    url: jdbc:h2:file:./data/chat-memory
    username: sa
    password:
  ai:
    chat:
      memory:
        repository:
          jdbc:
            initialize-schema: embedded
```

Use the auto-configured repository:

```java
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.ChatMemoryRepository;
import org.springframework.ai.chat.memory.MessageWindowChatMemory;
import org.springframework.context.annotation.Bean;

@Bean
ChatMemory persistentChatMemory(ChatMemoryRepository repository) {
    return MessageWindowChatMemory.builder()
            .chatMemoryRepository(repository)
            .maxMessages(20)
            .build();
}
```

For a managed production schema, set initialization to `never` and use Flyway or Liquibase.

```yaml
spring:
  ai:
    chat:
      memory:
        repository:
          jdbc:
            initialize-schema: never
```

Supported databases and behavior depend on the Spring AI release. Verify tool-message persistence if chat memory and tool calling are combined.

## 19. Tool Calling with Spring Methods

Tool calling allows a model to request a deterministic Java operation. The Java application executes the operation and returns its result to the model.

```text
User asks a question
      |
      v
Model requests a named tool with arguments
      |
      v
Spring AI validates and invokes the Java method
      |
      v
Tool result returns to the model
      |
      v
Model creates the final answer
```

Example tool:

```java
package com.example.javaai.tools;

import org.springframework.ai.tool.annotation.Tool;
import org.springframework.stereotype.Component;

@Component
public class CourseTools {

    @Tool(description = "Return the number of available seats for a course code")
    public SeatInfo availableSeats(String courseCode) {
        // A real application would call a validated business service.
        return switch (courseCode.toUpperCase()) {
            case "JAVA-BASIC" -> new SeatInfo(courseCode, 12);
            case "JDBC" -> new SeatInfo(courseCode, 5);
            default -> new SeatInfo(courseCode, 0);
        };
    }

    public record SeatInfo(String courseCode, int availableSeats) {
    }
}
```

Make the tool available for one request:

```java
public String answerWithTools(String question) {
    return chatClient.prompt()
            .user(question)
            .tools(courseTools)
            .call()
            .content();
}
```

Tool rules:

- Use clear names and descriptions.
- Use small serializable request and response types.
- Validate arguments inside application code.
- Apply the logged-in user's authorization.
- Make sensitive tools available only to requests that need them.
- Add timeouts and audit logging.
- Require confirmation for destructive or high-impact operations.
- Never allow the model to execute arbitrary SQL, shell commands, or Java code.

The model chooses whether to request a tool, but the application remains responsible for permission and execution.

## 20. Embeddings with EmbeddingModel

An embedding converts text into a numeric vector that represents meaning.

Spring AI exposes the portable `EmbeddingModel` interface.

```java
import org.springframework.ai.embedding.EmbeddingModel;
import org.springframework.stereotype.Service;

@Service
public class EmbeddingService {

    private final EmbeddingModel embeddingModel;

    public EmbeddingService(EmbeddingModel embeddingModel) {
        this.embeddingModel = embeddingModel;
    }

    public float[] embed(String text) {
        return embeddingModel.embed(text);
    }
}
```

Use embeddings for:

- Semantic search
- Similarity comparison
- Document clustering
- Duplicate detection
- RAG retrieval
- Recommendation features

```text
"JDBC connects Java to databases"
                 |
                 v
            EmbeddingModel
                 |
                 v
[0.018, -0.224, 0.791, ...]
```

Vectors from different embedding models may have different dimensions and meanings. Do not index documents with one embedding model and query them with an incompatible model.

## 21. Documents and VectorStore

Spring AI represents knowledge items with `Document` objects.

A document contains:

- Text
- Metadata
- An identifier
- Optional media information, depending on the use case

```java
import org.springframework.ai.document.Document;

Document jdbcNote = new Document(
        "PreparedStatement supports parameterized SQL and helps prevent SQL injection.",
        Map.of(
                "topic", "jdbc",
                "level", "beginner",
                "source", "trainer-notes"));
```

`VectorStore` provides a portable interface for storing and searching documents.

```java
vectorStore.add(List.of(jdbcNote));

List<Document> matches = vectorStore.similaritySearch(
        SearchRequest.builder()
                .query("How can JDBC avoid SQL injection?")
                .topK(3)
                .build());
```

Metadata enables filtering:

```java
SearchRequest request = SearchRequest.builder()
        .query("Explain prepared statements")
        .topK(5)
        .filterExpression("level == 'beginner'")
        .build();
```

Vector search finds semantically similar documents. It does not prove that a document is correct, current, or authorized for the user.

`SimpleVectorStore` is useful for learning and tests, not production deployment.

## 22. ETL for AI Documents

The Spring AI ETL pipeline prepares documents for a vector store.

```text
Extract                 Transform                 Load
Reader                  Split / enrich            VectorStore
PDF, text, JSON   →     Document chunks     →     Stored vectors
```

Main interfaces:

| Interface | Java shape | Responsibility |
| --- | --- | --- |
| `DocumentReader` | `Supplier<List<Document>>` | Reads source content |
| `DocumentTransformer` | `Function<List<Document>, List<Document>>` | Splits or enriches documents |
| `DocumentWriter` | `Consumer<List<Document>>` | Writes the processed documents |

Example using a PDF reader:

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-pdf-document-reader</artifactId>
</dependency>
```

```java
PagePdfDocumentReader reader = new PagePdfDocumentReader(
        new ClassPathResource("java-handbook.pdf"));

TokenTextSplitter splitter = new TokenTextSplitter();

List<Document> pages = reader.read();
List<Document> chunks = splitter.apply(pages);
vectorStore.add(chunks);
```

In Spring AI 1.1.x, the constructor form of `TokenTextSplitter` is appropriate for this release line. Later major versions may prefer a builder.

Chunking affects retrieval quality. Chunks that are too large contain unrelated information; chunks that are too small lose context.

Keep useful metadata such as:

- Source file
- Page number
- Heading
- Document version
- Tenant or organization
- Access classification
- Ingestion date

Never load an untrusted user-supplied URL directly into a document reader without protection against server-side request forgery.

## 23. Retrieval-Augmented Generation

RAG retrieves relevant application data and adds it to the model request.

```text
Question
   |
   v
Embedding + similarity search
   |
   v
Relevant documents
   |
   v
Question + retrieved context
   |
   v
Chat model → grounded answer
```

Add the advisor module when it is not already present transitively:

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-vector-store-advisor</artifactId>
</dependency>
```

Configure a RAG client:

```java
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.vectorstore.QuestionAnswerAdvisor;
import org.springframework.ai.vectorstore.VectorStore;

@Bean
ChatClient ragChatClient(
        ChatClient.Builder builder,
        VectorStore vectorStore) {

    return builder
            .defaultSystem("""
                    Answer using the supplied context.
                    If the context does not contain the answer, say that you do not know.
                    """)
            .defaultAdvisors(
                    QuestionAnswerAdvisor.builder(vectorStore).build())
            .build();
}
```

Call it normally:

```java
public String answerFromNotes(String question) {
    return ragChatClient.prompt()
            .user(question)
            .call()
            .content();
}
```

RAG has two separate workflows:

1. **Indexing:** Read, clean, split, embed, and store documents.
2. **Retrieval:** Embed a question, search for similar chunks, and send selected context to the chat model.

RAG reduces some hallucinations, but does not guarantee truth. Evaluate retrieval and generation separately.

## 24. PGvector for a Production-Style RAG Application

PGvector adds vector similarity search to PostgreSQL.

Add the starter:

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-pgvector</artifactId>
</dependency>

<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

Development configuration:

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/java_ai
    username: postgres
    password: ${DB_PASSWORD}
  ai:
    vectorstore:
      pgvector:
        index-type: HNSW
        distance-type: COSINE_DISTANCE
        initialize-schema: true
```

Important points:

- PGvector requires the PostgreSQL vector extension.
- The embedding dimension must match the selected embedding model.
- Schema initialization is opt-in.
- Use migrations and `initialize-schema: false` in controlled production environments.
- Choose an index and distance type deliberately.
- Store tenant and access metadata for retrieval filtering.

Never retrieve documents solely by similarity in a multi-tenant application. Add an authorization filter so a user can retrieve only permitted documents.

## 25. Exception Handling and Resilience

AI calls can fail because of:

- Invalid credentials
- Rate limits
- Provider downtime
- Network timeouts
- Unsupported models or options
- Context-window limits
- Malformed structured output
- Tool failures
- Vector database failures

Create a safe API error type:

```java
public record ApiError(
        String code,
        String message,
        Instant timestamp) {
}
```

```java
@RestControllerAdvice
public class ApiExceptionHandler {

    @ExceptionHandler(IllegalArgumentException.class)
    ResponseEntity<ApiError> handleBadInput(IllegalArgumentException ex) {
        ApiError error = new ApiError(
                "INVALID_REQUEST",
                ex.getMessage(),
                Instant.now());
        return ResponseEntity.badRequest().body(error);
    }

    @ExceptionHandler(Exception.class)
    ResponseEntity<ApiError> handleUnexpected(Exception ex) {
        ApiError error = new ApiError(
                "AI_SERVICE_ERROR",
                "The AI service is temporarily unavailable.",
                Instant.now());
        return ResponseEntity.status(503).body(error);
    }
}
```

Do not return provider stack traces, API keys, raw prompts, or internal URLs to clients.

Retry only transient failures. Use exponential backoff and a small retry limit. Do not retry invalid credentials, invalid requests, or every malformed answer indefinitely.

For operations that can cause side effects, add idempotency controls before retrying.

## 26. Validation and API Design

Validate the request before spending tokens.

```java
public record AskRequest(
        @NotBlank
        @Size(max = 4000)
        String question,

        @Pattern(regexp = "beginner|intermediate")
        String level) {
}
```

Good API practices:

- Use `POST` for prompts that contain sensitive or long content.
- Limit request size.
- Authenticate callers.
- Apply rate limits and quotas.
- Use server-generated conversation IDs or verify ownership.
- Return stable DTOs rather than raw provider responses.
- Add request IDs for tracing.
- Document whether an endpoint streams.
- Define timeouts.

Do not place prompts in query parameters when they may contain confidential information. URLs are commonly logged by browsers, proxies, and servers.

## 27. Observability and Logging

Spring AI integrates with Spring Boot Actuator and Micrometer observations.

It can observe components such as:

- `ChatClient`
- Advisors
- `ChatModel`
- `EmbeddingModel`
- Tool calls
- `VectorStore`

Useful production measurements:

| Measurement | Why it matters |
| --- | --- |
| Request count | Shows usage volume |
| Latency | Reveals slow providers or retrieval |
| Error count | Identifies reliability problems |
| Input and output tokens | Supports cost and context monitoring |
| Model name | Explains behavior and cost differences |
| Tool latency | Finds slow business operations |
| Vector-store latency | Finds retrieval bottlenecks |

Actuator health endpoint:

```http
GET /actuator/health
```

Prompt and completion content is not logged by default because it may be large and sensitive. Keep that safe default in production.

If detailed logging is temporarily enabled:

- Use a non-production environment when possible.
- Redact personal and secret data.
- Set a short retention period.
- Restrict log access.
- Disable the setting after diagnosis.

## 28. Testing and Evaluation

Generative AI testing has deterministic and probabilistic parts.

### Unit Tests

Unit-test normal Java logic without calling a real model:

- Input validation
- Prompt variable preparation
- Controller behavior
- Tool authorization
- Output validation
- Metadata filters
- Fallback selection

### Integration Tests

Use a small, controlled suite to verify:

- Provider authentication
- Model configuration
- Structured output mapping
- Streaming behavior
- Vector-store integration
- Tool calling

Do not make every build depend on a paid external model. Separate fast unit tests from opt-in integration tests.

### Evaluation Tests

Create a dataset of representative questions and expected qualities.

```text
Question | Expected facts | Forbidden claims | Required source | Score
```

Evaluate:

- Relevance
- Factual grounding
- Completeness
- Citation correctness
- Safety
- Format compliance
- Latency
- Token cost

Spring AI provides evaluators such as `RelevancyEvaluator` and `FactCheckingEvaluator`. An evaluator that uses an LLM is still probabilistic, so combine it with human review and deterministic assertions.

Example deterministic assertions for a quiz:

```java
assertNotNull(quiz);
assertEquals(3, quiz.questions().size());
assertTrue(quiz.questions().stream()
        .allMatch(question -> question.choices().size() == 4));
assertTrue(quiz.questions().stream()
        .allMatch(question -> question.correctChoiceIndex() >= 0
                && question.correctChoiceIndex() < question.choices().size()));
```

## 29. Security and Responsible AI

Treat model input, retrieved documents, tool results, and model output as untrusted data.

Major risks:

| Risk | Example | Control |
| --- | --- | --- |
| Prompt injection | A document says to ignore system rules | Separate data from instructions; restrict tools |
| Data leakage | One user's history reaches another user | Verify conversation ownership and tenant filters |
| Excessive agency | The model can cancel any order | Narrow tool permissions and require confirmation |
| Hallucination | The model invents a policy | Use RAG, citations, validation, and human review |
| Secret exposure | API key is committed | Use environment variables or a secrets manager |
| Unsafe output | Generated code contains vulnerabilities | Scan and review before use |
| Denial of wallet | Automated requests consume many tokens | Rate limits, budgets, quotas, and output limits |

Security boundaries must be enforced by Java code, not by a prompt.

```text
Prompt: "Only administrators may call this tool"   ← guidance only

Java authorization check before tool execution      ← real control
```

Before sending content to a provider:

- Remove unnecessary personal data.
- Confirm that data processing is permitted.
- Apply retention and regional requirements.
- Classify the information.
- Review provider data-use settings.

Before using generated output:

- Escape output displayed as HTML.
- Parameterize database queries.
- Never execute generated shell or Java code automatically.
- Validate URLs and filenames.
- Require approval for financial, legal, medical, or destructive actions.

## 30. Cost and Performance

AI response time normally includes more than model generation.

```text
Total latency = validation
              + retrieval
              + provider network time
              + model generation
              + tool execution
              + output validation
```

Cost and performance techniques:

- Select the smallest model that meets the quality requirement.
- Limit prompt and output sizes.
- Keep only useful memory.
- Retrieve a small number of high-quality chunks.
- Cache safe, repeatable results.
- Stream long responses.
- Batch embedding work where supported.
- Avoid sending identical system content unnecessarily.
- Set timeouts and cancellation rules.
- Track tokens by feature, tenant, and model.

Do not cache answers containing user-specific, permission-sensitive, or rapidly changing data without a correct cache key and expiry policy.

## 31. Model Context Protocol

Model Context Protocol, or MCP, is a standard for connecting AI applications to external tools and resources.

```text
Spring AI application (MCP client)
             |
             | Standard protocol
             v
MCP server exposing tools, resources, and prompts
```

Spring AI 1.1.x provides Boot starters for MCP clients and servers.

Examples:

```xml
<!-- MCP client -->
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-client</artifactId>
</dependency>
```

```xml
<!-- Web MVC MCP server -->
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

MCP is useful when tools should be shared across applications or languages. A normal in-process `@Tool` method is simpler when the tool belongs only to one Spring Boot application.

| In-process tool | MCP tool |
| --- | --- |
| Runs in the same application | Runs behind a protocol boundary |
| Simple setup | Supports reusable external capability servers |
| Shares the application's lifecycle | Has its own connection and lifecycle concerns |
| Suitable for local business services | Suitable for cross-application integrations |

MCP does not remove security requirements. Authenticate connections, authorize each capability, validate arguments, restrict network access, and audit sensitive operations.

## 32. Suggested Package Structure

```text
com.example.javaai
├── JavaAiApplication.java
├── config
│   ├── AiConfiguration.java
│   └── SecurityConfiguration.java
├── chat
│   ├── JavaTutorController.java
│   ├── JavaTutorService.java
│   ├── AskRequest.java
│   └── AskResponse.java
├── quiz
│   ├── QuizController.java
│   ├── QuizService.java
│   └── JavaQuiz.java
├── rag
│   ├── DocumentIngestionService.java
│   └── KnowledgeService.java
├── tools
│   └── CourseTools.java
├── error
│   ├── ApiError.java
│   └── ApiExceptionHandler.java
└── evaluation
    └── KnowledgeEvaluationTest.java
```

Organize by feature when the application grows. The important principle is separation of responsibilities, not a particular folder name.

## 33. Mini-Project: Java Learning Assistant

Build the application in stages.

### Stage 1: Basic Chat

- Create a Spring Boot 3.5.16 project.
- Import Spring AI 1.1.8 BOM.
- Add the OpenAI model starter.
- Create `/api/ai/ask`.
- Add request validation.

### Stage 2: Typed Quiz Generation

- Create `JavaQuiz` records.
- Use `.entity(JavaQuiz.class)`.
- Validate question count, choices, and correct indexes.

### Stage 3: Streaming

- Add WebFlux.
- Create an SSE endpoint.
- Handle client cancellation.

### Stage 4: Memory

- Add `MessageWindowChatMemory`.
- Require a conversation ID.
- Prevent cross-user conversation access.
- Add JDBC persistence.

### Stage 5: RAG

- Load the earlier Java and JDBC notes.
- Split them into chunks.
- Store embeddings in PGvector.
- Use `QuestionAnswerAdvisor`.
- Return source metadata with answers.

### Stage 6: Tools

- Add a read-only course-information tool.
- Validate course codes.
- Add authorization and audit logging.

### Stage 7: Production Readiness

- Add Actuator.
- Add timeouts, retry limits, and rate limits.
- Create an evaluation dataset.
- Record model, latency, token usage, and retrieval quality.
- Perform a prompt-injection review.

Suggested endpoints:

| Method | Path | Purpose |
| --- | --- | --- |
| `POST` | `/api/ai/ask` | Stateless question answering |
| `POST` | `/api/ai/quiz` | Typed quiz generation |
| `GET` | `/api/ai/stream` | Streaming explanation |
| `POST` | `/api/ai/conversations/{id}/messages` | Chat with memory |
| `POST` | `/api/ai/knowledge/ask` | RAG-based answers |

## 34. Common Errors

| Error or symptom | Likely cause | Correction |
| --- | --- | --- |
| No `ChatClient.Builder` bean | Model starter or configuration is missing | Add one model starter and valid provider settings |
| 401 or 403 from provider | Missing, invalid, or unauthorized key | Check the environment variable and provider account |
| Model not found | The configured model is unavailable | Use a model enabled for the provider account |
| Memory call throws `IllegalArgumentException` | Conversation ID was omitted | Pass `ChatMemory.CONVERSATION_ID` on every memory call |
| Different users share context | Conversation IDs are reused or not authorized | Generate unique IDs and verify ownership |
| `.entity()` fails | Model output does not match the target schema | Simplify the type, improve the prompt, validate, and retry selectively |
| Streaming endpoint buffers everything | Wrong content type or reactive support missing | Use SSE and include WebFlux support |
| RAG returns irrelevant chunks | Poor chunking, embeddings, or search settings | Tune chunks, metadata, top-k, and similarity threshold |
| PGvector table is missing | Schema initialization is disabled | Enable it for development or run a migration |
| Tool receives unsafe values | Tool arguments were trusted | Validate and authorize in Java code |
| Logs expose prompts | Detailed logging was enabled | Disable it and redact sensitive data |
| Cost increases unexpectedly | Unbounded prompts, output, memory, or retries | Add limits, quotas, monitoring, and budgets |

## 35. Frequently Asked Interview Questions

### What Is Spring AI?

Spring AI is a Spring project that provides portable Java abstractions and Spring Boot integrations for AI models, embeddings, vector stores, tools, chat memory, RAG, and related application patterns.

### Does Spring AI Provide an LLM?

No. It connects applications to models hosted by providers or running locally.

### What Is the Difference Between ChatModel and ChatClient?

`ChatModel` is the lower-level model interface. `ChatClient` is a higher-level fluent API for prompts, advisors, tools, streaming, and typed output.

### What Does a Spring AI Starter Do?

A starter brings the required integration dependencies and enables Spring Boot auto-configuration for a model provider, vector store, memory repository, or MCP component.

### Why Use the Spring AI BOM?

The BOM keeps Spring AI module versions consistent and allows individual Spring AI dependencies to omit explicit versions.

### How Should an API Key Be Stored?

Use an environment variable, container secret, or secrets manager. Never hard-code or commit it.

### What Is an Advisor?

An advisor intercepts and enriches AI requests or responses. It is commonly used for memory, RAG, logging, and reusable policies.

### Does an LLM Remember Previous REST Calls?

No. The application must store selected messages and send relevant memory with later requests.

### What Is the Difference Between Chat Memory and Chat History?

Chat memory is the selected context supplied to the model. Chat history is the complete conversation record stored for product or audit needs.

### Why Is a Conversation ID Required?

It identifies which stored messages belong to a conversation. It must also be associated with and authorized for the current user.

### What Does `.entity(SomeType.class)` Do?

It requests structured output and converts the result into the specified Java type. The result must still be validated.

### What Is Tool Calling?

Tool calling allows the model to request that the application invoke an approved Java operation with structured arguments.

### Who Authorizes a Tool Call?

The Java application authorizes it. A model or prompt is never the security boundary.

### What Is EmbeddingModel?

It is Spring AI's portable interface for converting content into numeric embedding vectors.

### What Is VectorStore?

It is Spring AI's abstraction for storing embedded documents and performing semantic similarity searches.

### What Is QuestionAnswerAdvisor?

It is a RAG advisor that retrieves relevant documents from a vector store and adds them as context to a chat request.

### Why Is SimpleVectorStore Not Recommended for Production?

It is designed for demonstrations and tests, not production durability, concurrency, security, and scale.

### Why Use PGvector?

It adds vector similarity search to PostgreSQL and lets an application combine relational data, metadata, and vector retrieval.

### What Is MCP?

MCP is a standard protocol through which AI applications can discover and use external tools, resources, and prompts.

### How Should AI Features Be Tested?

Use deterministic unit tests, controlled integration tests, evaluation datasets, automated evaluators, and human review.

### Does RAG Eliminate Hallucination?

No. RAG can improve grounding, but retrieval can be wrong and the model can still misinterpret or invent information.

### Why Monitor Tokens?

Token usage affects cost, context-window limits, response time, and the amount of information sent to the provider.

---

[Home](index.md) ▪️ [Part 1: Generative AI with Java](generative-ai-with-java.md)
