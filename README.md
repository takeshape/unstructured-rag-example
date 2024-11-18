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

