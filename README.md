# getUser() - GitHub API Documentation

## Purpose
Retrieves public profile information for a specified GitHub user by making a GET request to the GitHub Users API.

---

## Endpoint
```
GET https://api.github.com/users/{username}
```

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `username` | string | Yes | The GitHub username of the user to retrieve (e.g., `"KiryuuJ"`) |

---

## Return Values

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique identifier for the user |
| `login` | string | The user's GitHub username |
| `name` | string | The user's display name |
| `email` | string or null | Publicly visible email address |
| `public_repos` | integer | Number of public repositories |
| `followers` | integer | Number of followers |
| `created_at` | string | Account creation date (ISO 8601 format) |

---

## Example Usage
```javascript
async function getUser(username) {
  const response = await fetch(`https://api.github.com/users/${username}`);

  if (!response.ok) {
    throw new Error(`Error ${response.status}: ${response.statusText}`);
  }

  const user = await response.json();
  return user;
}

// Call the function
getUser("KiryuuJ")
  .then(user => console.log(user.name, user.public_repos))
  .catch(err => console.error(err));
```

### Sample Response
```json
{
  "login": "KiryuuJ",
  "id": 583231,
  "name": "KiryuuJ",
  "email": null,
  "public_repos": 1,
  "followers": 0,
  "created_at": "2024-01-01T00:00:00Z"
}
```

---

## Error Handling

| HTTP Status | Meaning | How to Handle |
|-------------|---------|---------------|
| `200 OK` | Success | Parse and use the JSON response |
| `404 Not Found` | Username does not exist | Notify the user the account was not found |
| `403 Forbidden` | Rate limit exceeded | Wait before retrying; consider authenticating |
| `503 Service Unavailable` | GitHub server issue | Retry after a short delay |

> **Note:** Unauthenticated requests are limited to **60 requests per hour**.
