### Farmgate Cloud API Quickstart Guide
Welcome to the Farmgate Cloud API Quickstart Guide! This guide will help you get up and running with the Farmgate Cloud API using curl.

## Prerequisites
Before you begin, ensure you have:

An API key provided by Farmgate Cloud.
curl installed on your machine. You can check by running:

curl --version

# Step 1: Test Your API Key
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

# Step 2: Fetch Your Organizations
Each user has access to two organizations:

1. Your Default Organization: Full read/write access.
2. Demo Organization: Read-only access.

To retrieve your list of organizations, run:

```
curl -X GET https://api.nighthawk.ag/organizations \
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