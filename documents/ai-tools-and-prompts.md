### **Detailed Documentation for AI Integrations & Prompts**

The **AI Integrations & Prompts** system enables the platform to integrate with various AI-powered services and tools. It provides users with an interface to trigger AI-related tasks, such as querying AI models, generating prompts, and utilizing AI tools for various purposes. The system is flexible, allowing for both pre-defined AI tool integrations and custom user-created prompts, providing a comprehensive and dynamic solution for interacting with AI technologies.

---

### **1. Features Overview**

- **AI Tool Integrations**: The system supports third-party AI integrations (e.g., GPT models, NLP services, machine learning tools).
- **Custom AI Prompts**: Users can create custom prompts to interact with integrated AI services.
- **Predefined AI Tools**: The platform comes with a set of predefined AI tools and services that can be accessed out-of-the-box.
- **AI Prompt Management**: Users can manage, save, and reuse custom AI prompts.
- **Real-Time AI Responses**: Responses from AI tools can be integrated into real-time interactions with users.
- **AI Result Handling**: The platform allows handling AI-generated results, such as text, images, or predictions, in various formats.

---

### **2. Database Schema**

#### **AI Tools Table**
This table stores all AI tools integrated with the platform, including predefined and third-party tools.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| name              | String      | Name of the AI tool (e.g., "GPT-3", "Text Summarizer")        |
| description       | Text        | Description of the AI tool's functionality                    |
| type              | String      | Type of tool (e.g., "text_generation", "image_generation")    |
| api_url           | String      | URL for the AI tool API integration (if applicable)           |
| api_key           | String      | API key for accessing the AI tool (if applicable)             |
| created_at        | TIMESTAMP   | Timestamp when the AI tool was added                          |
| updated_at        | TIMESTAMP   | Timestamp when the AI tool details were last updated          |

#### **AI Prompts Table**
This table stores user-defined prompts for interacting with AI tools.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| user_id           | UUID        | Foreign key referencing `users.id`                            |
| ai_tool_id        | UUID        | Foreign key referencing `ai_tools.id`                         |
| prompt            | Text        | The actual prompt or query that will be sent to the AI tool   |
| response_format   | String      | The expected format of the response (e.g., "text", "json")    |
| result            | Text        | Stored AI response from the prompt (optional, if saved)       |
| created_at        | TIMESTAMP   | Timestamp when the prompt was created                         |
| updated_at        | TIMESTAMP   | Timestamp when the prompt was last updated                    |

#### **AI Prompt History Table**
This table stores the history of executed AI prompts, including the input prompt and AI-generated responses.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| ai_prompt_id      | UUID        | Foreign key referencing `ai_prompts.id`                       |
| input_prompt      | Text        | The input prompt/query sent to the AI tool                    |
| ai_response       | Text        | Response from the AI tool                                     |
| status            | Enum        | Status of the prompt execution (`success`, `failed`)          |
| created_at        | TIMESTAMP   | Timestamp when the history entry was created                  |
| updated_at        | TIMESTAMP   | Timestamp when the history entry was last updated             |

#### **AI Tool Categories Table**
This table categorizes AI tools for better organization and easier discovery.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| name              | String      | Name of the category (e.g., "NLP", "Machine Learning")        |
| description       | Text        | Description of the category                                    |
| created_at        | TIMESTAMP   | Timestamp when the category was created                        |
| updated_at        | TIMESTAMP   | Timestamp when the category was last updated                   |

---

### **3. Relationships & Pivot Tables**

- **Users & AI Prompts**  
  One-to-many relationship: Each user can have many AI prompts, but each prompt belongs to one user.

- **AI Tools & AI Prompts**  
  One-to-many relationship: Each AI tool can have many prompts, but each prompt is associated with one AI tool.

- **AI Tools & AI Tool Categories**  
  Many-to-one relationship: Each AI tool belongs to one category, but each category can have multiple AI tools.

- **AI Prompts & AI Prompt History**  
  One-to-many relationship: Each AI prompt can have many historical records, but each history record is related to one prompt.

---

### **4. API Endpoints**

#### **AI Tools Management**

- **GET /api/ai-tools**: Retrieve a list of all AI tools available on the platform.
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "name": "GPT-3",
        "description": "Generative Pretrained Transformer for natural language processing",
        "type": "text_generation",
        "api_url": "https://api.openai.com/v1/completions"
      }
    ]
    ```

- **POST /api/ai-tools**: Add a new AI tool to the platform.
  - Request Body:
    ```json
    {
      "name": "GPT-3",
      "description": "Generative Pretrained Transformer for natural language processing",
      "type": "text_generation",
      "api_url": "https://api.openai.com/v1/completions",
      "api_key": "your-api-key"
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "AI tool added successfully"
    }
    ```

#### **AI Prompts Management**

- **GET /api/ai-prompts**: Retrieve a list of all AI prompts created by the authenticated user.
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "ai_tool_id": "uuid",
        "user_id": "uuid",
        "prompt": "What is the weather like in New York?",
        "response_format": "text",
        "created_at": "2025-04-01T10:00:00Z"
      }
    ]
    ```

- **POST /api/ai-prompts**: Create a new AI prompt and execute it with the selected AI tool.
  - Request Body:
    ```json
    {
      "ai_tool_id": "uuid",
      "user_id": "uuid",
      "prompt": "What is the capital of France?",
      "response_format": "text"
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "Prompt executed successfully",
      "ai_response": "The capital of France is Paris."
    }
    ```

#### **AI Prompt History**

- **GET /api/ai-prompts/history**: Retrieve the execution history of AI prompts for the authenticated user.
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "ai_prompt_id": "uuid",
        "input_prompt": "What is the weather like in New York?",
        "ai_response": "The weather in New York is sunny and 22°C.",
        "status": "success",
        "created_at": "2025-04-01T10:00:00Z"
      }
    ]
    ```

- **POST /api/ai-prompts/history**: Manually execute an AI prompt and save the result in the history.
  - Request Body:
    ```json
    {
      "ai_prompt_id": "uuid",
      "input_prompt": "What is the tallest building in the world?",
      "ai_response": "The tallest building in the world is the Burj Khalifa."
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "Prompt history saved successfully"
    }
    ```

---

### **5. AI Tool Categories**

- **GET /api/ai-tool-categories**: Retrieve a list of all AI tool categories available on the platform.
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "name": "NLP",
        "description": "Natural Language Processing tools"
      }
    ]
    ```

- **POST /api/ai-tool-categories**: Create a new AI tool category.
  - Request Body:
    ```json
    {
      "name": "Machine Learning",
      "description": "Tools for building and deploying machine learning models"
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "AI tool category created successfully"
    }
    ```

---

### **6. Summary of Key Features:**

- **AI Tool Integration**: Integrate various AI tools for text generation, image generation, etc.
- **Custom AI Prompts**: Users can create and save custom prompts to interact with AI services.
- **Prompt History**: Track the history of AI prompt executions, including the input and output.
- **Categorization**: Organize AI tools into categories for better discoverability.
- **Real-time AI Responses**: AI-generated responses can be instantly provided to users.

---

This concludes the documentation for **AI Integrations & Prompts**. If you need further information or clarification, feel free to reach out!