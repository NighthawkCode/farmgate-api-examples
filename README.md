# Farmgate Cloud API Quickstart Guide
Welcome to the Farmgate Cloud API Quickstart Guide! This guide will help you get up and running with the Farmgate Cloud API using curl.

## Pre-requisites
Before you begin, ensure you have:

- An API key provided by Farmgate Cloud.
- curl installed on your machine. You can check by running:

```
curl --version
```

## Step 1: Test Your API Key
Your API key is required to authenticate requests. Let’s verify your API key with a simple health check endpoint.

Run the following curl command (replace <YOUR_API_KEY> with your actual key):

```
curl -X GET https://api.nighthawk.ag/health \
-H "Authorization: Bearer <YOUR_API_KEY>"
```

If your API key is valid, you should receive a response like:

```
{
  "status": "healthy",
  "message": "API is up and running!"
}
```

If you encounter any issues, double-check your API key and contact support.

## Step 2: Fetch Your User Information
Before interacting with other endpoints, you need to fetch your user information. This user information object contains key details about your account, including your user_id, email, and the list of organizations associated with your account.

To retrieve your user information, run the following command:

```
curl -X GET https://api.nighthawk.ag/farmgate/get-user-info \
-H "Authorization: Bearer <YOUR_API_KEY>"
```

You will receive a JSON object similar to the following:

```
{
    "user_info": {
        "user_id": "USER_12345",
        "user_email": "user@example.com"
    },
    "organizations": [
        {
            "id": "ID_1",
            "name": "Default Organization"
        },
        {
            "id": "ID_2",
            "name": "Demo Organization"
        }
    ]
}
```

## Step 3: List Documents
Now that you have your user_id and organization details, you can interact with specific organizations by providing their organization_id. For example, you can list documents associated with an organization. Use the Demo Organization to explore some example documents.

### List All Documents

Run the following command (replace <ORG_ID> with the organization ID you retrieved earlier):

```
curl -X POST https://api.nighthawk.ag/farmgate/<ORG_ID>/documents \
-H "Authorization: Bearer <YOUR_API_KEY>" \
-H "Content-Type: application/json" \
-d '{}'
```

### Optional Parameters for Filtering and Sorting
- **name_str:** Filter documents by a partial or full name (e.g., "invoice").
- **sort_by:** Sort results by a field: "name", "type", "upload_date"
- **sort_order:** Order results ascending ("asc") or descending ("desc"). Default: descending
- **limit:** The maximum number of documents to return (-1 or blank for no limit).
- **offset:** Skip the first n results for pagination

```
curl -X POST https://api.nighthawk.ag/farmgate/<ORG_ID>/documents \
-H "Authorization: Bearer <YOUR_API_KEY>" \
-H "Content-Type: application/json" \
-d '{
  "name_str": "invoice",
  "sort_by": "upload_date",
  "limit": 10,
  "offset": 0
}'
```

## Step 4: Speak with Copilot
Copilot is Farmgate's AI assistant designed to help farmers manage their operations effectively by combining the power of AI with your farm's data. It can answer questions, process documents, and perform advanced operations based on your queries.

To interact with the Copilot, use the /farmgate/copilot endpoint, which supports streaming responses for a conversational experience.

### Initiating a Conversation with Copilot
To start a conversation with the Copilot, send a POST request to the following endpoint.

curl -X POST https://api.nighthawk.ag/farmgate/copilot \
-H "Authorization: Bearer <YOUR_API_KEY>" \
-H "Content-Type: application/json" \
-d '{
  "user_email": <YOUR_USER_EMAIL>,
  "org_id": <YOUR_ORG_ID>,
  "message": "Hello Copilot!",
}'

Request Parameters:

user_email: (Required) The email of the user initiating the conversation.
org_id: (Required) The organization ID for which the query is being made.
message: (Required) The user’s question or query.
chat_id: (Optional) The chat session ID. If not provided, a new chat session will be started.

### Example Response
The Copilot will respond with a streaming response, which can be consumed line-by-line.