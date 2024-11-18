# Unstructured RAG Example
<a href="https://app.takeshape.io/add-pattern?repo=https://github.com/takeshape/unstructured-rag-example"><img alt="Deploy To TakeShape" src="https://images.takeshape.io/2cccc825-70be-431c-9ba0-10ab38ecd3a7/dev/8e2f7bda-0e08-4ede-a546-6df59be6a8bb/Deploy%20to%20TakeShape%402x.png?auto=format%2Ccompress" width="205" height="38" style="max-width:100%;"></a>


## Instructions

### Create a new TakeShape Project
1. Click [Deploy to TakeShape](https://app.takeshape.io/add-pattern?repo=https://github.com/takeshape/unstructured-rag-example).
1. Select "Create new project" from the dropdown
1. Enter a name for the new project or leave the default
1. Click "Add to TakeShape"

### Add your API keys

#### Unstructured 


Follow the directions in the [TakeShape Unstructured documentation](https://app.takeshape.io/docs/services/providers/unstructured) to connect Unstructured API


#### OpenAI

1. Create an OpenAI API Key https://platform.openai.com/api-keys with the "Models" and "Model capabilities" permissions (this example uses `/v1/embeddings` and `/v1/chat/completions`)
2. Copy/paste your API key into the service configuration dialog
3. Click "Save"

### Try it out!
Once your services are connected now try out the API in the API Explorer

```graphql
mutation {
  chat(input: "what shoes should I buy?")
}
```

```graphql
query {
  getRelatedDocumentList(text: "what shoes should I buy?") {
    items {
      filename
      chunks {
        text
      }
    }
    total
  }
}
```

## How it works

This example demonstrates how to use TakeShape's vector capabilities combined with indexing to enable the RAG use-case. A prerequisite for RAG is to populate a vector database, for this example we will use TakeShape's built-in index. The first step to preparing our data is to create the `Document` which extends the built-in Asset shape from TakeShape but adds `chunks` an array of text chunks and their corresponding vector embeddings.
```mermaid
sequenceDiagram
    participant TakeShape
    participant Unstructured
    participant OpenAI
    participant Index

    TakeShape->>Unstructured: 1. Assets uploaded to TakeShape are sent to Unstructured
    Unstructured->>TakeShape: 2. Unstructured parses and chunks the documents
    TakeShape->>OpenAI: 3. Create embeddings the text chunks
    OpenAI->>TakeShape: 4. OpenAI returns embeddings
    TakeShape->>Index: 5. Text chunks and embeddings in TakeShape Index
```

Now that our `Document` data is stored in the built-in index we can perform RAG:
```mermaid
sequenceDiagram
    actor User
    participant TakeShape
    participant Index
    participant OpenAI

    User->>TakeShape: 1. GraphQL containing user prompt
    TakeShape->>OpenAI: 2. Create embedding of user prompt
    OpenAI->>TakeShape: 3. OpenAI returns vector
    TakeShape->>Index: 4. Use vector to search TakeShape Index
    Index->>TakeShape: 5. Return related products
    TakeShape->>OpenAI: 6. Combine related results with original prompt and send to GPT 4o
    OpenAI->>TakeShape: 7. OpenAI returns generated text
    TakeShape->>User: 8. GraphQL response
```
