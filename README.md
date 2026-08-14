# Generative AI Learning Path for Java Freshers

This repository provides beginner-friendly reference material for teaching Generative AI to Java freshers. The content follows a two-part progression: first using Java 21 without Spring, and then applying the same concepts with Spring Boot and Spring AI.

## Start Here

[Open the course landing page](index.md)

```text
Generative AI Foundations
          |
          v
Core Java AI Integration
          |
          v
Spring Boot and Spring AI Integration
          |
          v
Memory, Tools, Embeddings, RAG, Testing, and Production Practices
```

## Course Modules

### Part 1: Generative AI with Core Java

[Read the Core Java notes](generative-ai-notes-for-java-freshers.md)

This module introduces:

- Artificial Intelligence, Machine Learning, Deep Learning, and Generative AI
- Large Language Models and tokens
- Prompts, message roles, and prompt templates
- Prompt-engineering techniques
- Model parameters and context windows
- Structured output and Java records
- Embeddings and vector databases
- Retrieval-Augmented Generation
- Tool calling
- Calling an AI API with Java 21 `HttpClient`
- Asynchronous and streaming responses
- Conversation memory
- Evaluation, security, cost, and performance

### Part 2: Generative AI with Spring

[Read the Spring AI notes](generative-ai-with-spring-notes-for-java-freshers.md)

This module continues with:

- Spring Boot and Spring AI architecture
- Spring AI BOM and model starters
- Auto-configuration and dependency injection
- `ChatModel` and `ChatClient`
- REST endpoints and request validation
- Prompt templates and model options
- Structured output into Java records
- Streaming with Reactor
- Advisors and conversation memory
- Persistent chat memory with JDBC
- Tool calling using Spring methods
- `EmbeddingModel`, `Document`, and `VectorStore`
- Document ETL and chunking
- RAG with `QuestionAnswerAdvisor`
- PGvector integration
- Exception handling and resilience
- Observability with Actuator and Micrometer
- Testing, evaluation, security, and MCP

## Technology Baseline

| Technology | Version or approach |
| --- | --- |
| Java | Java 21 |
| Core Java HTTP integration | Java `HttpClient` |
| Build tool | Maven |
| Spring Boot | 3.5.16 |
| Spring AI | 1.1.8 |
| Example AI provider | OpenAI |
| Example vector database | PostgreSQL with PGvector |

> Spring AI `1.1.18` is not a published release. The Spring module therefore uses the published Spring AI `1.1.8` version.

## Target Audience

The material is suitable for:

- Java freshers
- Learners who understand basic Java syntax and object-oriented programming
- Students beginning AI application development
- Trainers delivering Java and Generative AI courses
- Developers progressing from manual HTTP integration to Spring AI

## Prerequisites

Learners should understand:

- Java classes, objects, interfaces, and records
- Collections and exception handling
- Basic JSON
- Maven fundamentals
- REST API fundamentals
- Environment variables
- Basic SQL and JDBC before studying persistent chat memory or PGvector

Spring fundamentals are helpful before starting Part 2, especially dependency injection, Spring beans, configuration, services, and REST controllers.

## Learning Outcomes

After completing both parts, learners should be able to:

- Explain essential Generative AI concepts using Java terminology.
- Integrate a Java application with an AI model API.
- Design system messages, user messages, and prompt templates.
- Convert generated output into validated Java objects.
- Stream generated responses.
- Maintain conversation context safely.
- Expose approved Java operations as AI tools.
- Create embeddings and perform semantic searches.
- Explain and implement a basic RAG workflow.
- Integrate Spring Boot applications with Spring AI.
- Apply validation, security, evaluation, observability, and cost controls.

## Repository Files

```text
.
├── README.md
├── generative-ai-java-index.md
├── generative-ai-notes-for-java-freshers.md
└── generative-ai-with-spring-notes-for-java-freshers.md
```

| File | Purpose |
| --- | --- |
| `README.md` | Repository overview and usage guide |
| `generative-ai-java-index.md` | Course landing page and learning path |
| `generative-ai-notes-for-java-freshers.md` | Part 1 using Java 21 without Spring |
| `generative-ai-with-spring-notes-for-java-freshers.md` | Part 2 using Spring Boot and Spring AI |

## Recommended Teaching Sequence

1. Introduce the terminology and model workflow from Part 1.
2. Demonstrate prompts, structured output, embeddings, RAG, and tools conceptually.
3. Review the Java `HttpClient` integration to reveal the underlying HTTP workflow.
4. Begin Part 2 and map each Core Java responsibility to a Spring AI abstraction.
5. Progress from `ChatClient` to streaming, memory, tools, and RAG.
6. Finish with evaluation, security, observability, and the Java Learning Assistant mini-project.

## Using the Notes

- Begin from the [course landing page](generative-ai-java-index.md).
- Follow the modules in order.
- Use the navigation links at the top and bottom of each module.
- Run code fragments only after adding the required dependencies and configuration.
- Store API keys in environment variables or a secrets manager.
- Verify model names and provider capabilities before running an example.
- Treat every model response as untrusted data that requires validation.

## Navigation

[Home](index.md)  |  [Part 1: Generative AI with Java](generative-ai-with-java.md)  |  [Part 2: Generative AI with Spring](generative-ai-with-spring.md)
