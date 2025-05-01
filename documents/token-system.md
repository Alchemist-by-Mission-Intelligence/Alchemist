### **AI Tools Usage Token System for Alchemist AI**

The **AI Tools Usage Token System** is a critical feature of the Alchemist AI platform that allows users to interact with AI tools by consuming tokens. These tokens are purchased through pricing plans and are required to access and use various AI services integrated into the platform.

The token system is designed to manage AI tool usage efficiently, ensuring that users can track their consumption and purchase additional tokens if necessary. This system ensures that AI services are fairly priced, and users can scale their usage based on their needs.

---

### **1. Features Overview**

- **Token System**: Users buy tokens through predefined pricing plans.
- **AI Tools Usage**: Tokens are consumed when using AI tools or generating AI responses (e.g., text generation, image generation).
- **Token Balances**: Users can check their token balance and track token consumption.
- **Pricing Plans**: Pricing plans offer different amounts of tokens at varying price points.
- **Token Consumption**: Each AI tool or service consumes a specific number of tokens based on the complexity of the task.

---

### **2. Database Schema**

#### **Tokens Table**
This table stores information about AI usage tokens, including purchase history, token quantities, and expiration dates (if applicable).

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| user_id           | UUID        | Foreign key referencing `users.id`                            |
| amount            | Integer     | Number of tokens purchased                                    |
| price             | Decimal     | Price of the tokens (in the platform’s currency)              |
| status            | Enum        | Status of the transaction (`pending`, `completed`, `failed`)   |
| transaction_id    | UUID        | Reference to the transaction for auditing                     |
| expiration_date   | TIMESTAMP   | Expiration date for the tokens (if applicable)                |
| created_at        | TIMESTAMP   | Timestamp when the token purchase was made                    |
| updated_at        | TIMESTAMP   | Timestamp when the token purchase was last updated            |

#### **AI Tool Usage Table**
This table tracks token consumption when users interact with AI tools.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| user_id           | UUID        | Foreign key referencing `users.id`                            |
| ai_tool_id        | UUID        | Foreign key referencing `ai_tools.id`                         |
| tokens_consumed   | Integer     | Number of tokens consumed for the AI service                  |
| operation         | String      | Description of the AI tool operation (e.g., "text generation") |
| result            | Text        | AI response (optional, if saved)                              |
| status            | Enum        | Status of the operation (`success`, `failed`)                 |
| created_at        | TIMESTAMP   | Timestamp when the operation was executed                     |
| updated_at        | TIMESTAMP   | Timestamp when the operation was last updated                 |

#### **Pricing Plans Table**
This table stores different pricing plans that users can purchase to buy tokens.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| name              | String      | Name of the pricing plan (e.g., "Basic", "Pro", "Enterprise") |
| token_amount      | Integer     | Number of tokens provided with the plan                       |
| price             | Decimal     | Price of the plan                                              |
| billing_cycle     | String      | Billing cycle (`monthly`, `yearly`, etc.)                     |
| created_at        | TIMESTAMP   | Timestamp when the pricing plan was created                   |
| updated_at        | TIMESTAMP   | Timestamp when the pricing plan was last updated              |

#### **Token Purchases Table**
This table records the purchases of AI usage tokens by users through pricing plans.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| user_id           | UUID        | Foreign key referencing `users.id`                            |
| plan_id           | UUID        | Foreign key referencing `pricing_plans.id`                    |
| tokens_purchased  | Integer     | Number of tokens purchased                                    |
| amount_spent      | Decimal     | Amount spent on the purchase                                  |
| payment_status    | Enum        | Status of the payment (`pending`, `completed`, `failed`)       |
| transaction_id    | UUID        | Transaction ID for payment tracking                           |
| created_at        | TIMESTAMP   | Timestamp when the token purchase was made                    |
| updated_at        | TIMESTAMP   | Timestamp when the token purchase was last updated            |

---

### **3. Relationships & Pivot Tables**

- **Users & Tokens**  
  One-to-many relationship: A user can have many token purchases, but each token purchase belongs to one user.

- **Users & AI Tool Usage**  
  One-to-many relationship: A user can have many AI tool usages, but each usage is related to one user.

- **AI Tools & AI Tool Usage**  
  One-to-many relationship: An AI tool can have many usages associated with it.

- **Pricing Plans & Token Purchases**  
  One-to-many relationship: A pricing plan can be purchased many times by different users, but each token purchase is associated with one pricing plan.

---

### **4. API Endpoints**

#### **Token Management**

- **GET /api/tokens/balance**: Retrieve the current token balance of the authenticated user.
  - Response:
    ```json
    {
      "status": "success",
      "tokens_balance": 1000
    }
    ```

- **POST /api/tokens/purchase**: Buy tokens using a pricing plan.
  - Request Body:
    ```json
    {
      "plan_id": "uuid",
      "payment_method": "credit_card",
      "transaction_id": "uuid"
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "Tokens purchased successfully",
      "tokens_purchased": 500
    }
    ```

#### **AI Tool Usage**

- **POST /api/ai-tools/{tool_id}/use**: Use an AI tool and consume tokens.
  - Request Body:
    ```json
    {
      "user_id": "uuid",
      "ai_tool_id": "uuid",
      "tokens_required": 10,
      "operation": "text_generation",
      "prompt": "What is the capital of France?"
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "AI tool used successfully",
      "ai_response": "The capital of France is Paris.",
      "tokens_consumed": 10
    }
    ```

#### **Pricing Plans**

- **GET /api/pricing-plans**: Retrieve a list of all pricing plans available for token purchase.
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "name": "Basic",
        "token_amount": 100,
        "price": 10.00,
        "billing_cycle": "monthly"
      }
    ]
    ```

- **POST /api/pricing-plans**: Create a new pricing plan.
  - Request Body:
    ```json
    {
      "name": "Basic",
      "token_amount": 100,
      "price": 10.00,
      "billing_cycle": "monthly"
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "Pricing plan created successfully"
    }
    ```

---

### **5. Token Usage Flow**

1. **Purchasing Tokens**: 
   - Users purchase tokens via pricing plans. They can buy tokens using various payment methods (credit card, PayPal, etc.).
   
2. **Token Consumption**: 
   - Users consume tokens when interacting with AI tools, such as generating text, processing images, etc.
   - Each AI tool consumes a specific number of tokens based on the complexity of the request.

3. **Balance Management**: 
   - Users can check their remaining token balance and purchase more tokens if needed.

4. **Token Expiration** (optional):
   - Tokens may have an expiration date depending on the pricing plan. Expired tokens are not available for use.

---

### **6. Key Features**

- **Flexible Token Pricing**: Users can purchase tokens through different pricing plans tailored to various usage needs.
- **Token Consumption Per Service**: Each AI tool or service consumes a defined amount of tokens based on the complexity of the task.
- **Real-time Token Usage**: Tokens are deducted in real-time when users interact with AI tools.
- **Transparent Billing**: Users can track token consumption and purchase history.

---

### **7. Summary**

The **AI Tools Usage Token System** is a comprehensive solution for managing token-based access to AI tools on the Alchemist AI platform. It allows users to purchase tokens through various pricing plans and consume those tokens when interacting with AI services. This system ensures a smooth and scalable experience for both users and administrators while maintaining transparency in token usage and purchases.

---
