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

## Step 2: Fetch Your Organizations
Each user has access to two organizations:

1. Your Default Organization: Full read/write access.
2. Demo Organization: Read-only access.

To retrieve your list of organizations, run:

```
curl -X GET https://api.nighthawk.ag/farmgate/organizations \
-H "Authorization: Bearer <YOUR_API_KEY>"
```

It will return a JSON list of your organizations

```
[
  {
    "name": "Default Organization",
    "id": "ID_1"
  },
  {
    "name": "Demo Organization",
    "id": "ID_2"
  }
]
```

The id field represents the unique identifier for each organization. You’ll need the organization_id for many subsequent requests.

## Step 3: List Documents
Now that you have your organization_id, you can list the documents associated with your organization. You can use the Demo Organization to look at some example documents.

List All Documents

```
curl -X POST https://api.nighthawk.ag/farmgate/<ORG_ID>/documents \
-H "Authorization: Bearer <YOUR_API_KEY>" \
-H "Content-Type: application/json" \
-d '{}'
```

Optional Parameters for Filtering and Sorting
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