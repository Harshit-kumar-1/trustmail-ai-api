# Trustmail AI — API Documentation

**Version:** v1 <br>
**Base URL:** https://api.domainame.in <br>
**Authentication:** Bearer API Key

Trustmail AI provides intelligent, privacy-first email verification with explainable trust signals and risk scoring.

---

## Authentication

All API requests require an API key provided via the `Authorization` header.

```http
Authorization: Bearer YOUR_API_KEY
```

Requests without a valid API key will be rejected.

---

## Content Type

All requests and responses must use JSON.

```http
Content-Type: application/json
```

---

## API Endpoints

### Verify Email

**POST** `/v1/email/verify`

Verifies an email address and returns trust signals, a risk score, and confidence level.

#### Why POST?

- Email addresses are sensitive data
- Prevents exposure in URLs and logs
- Cleaner and more secure request handling

---

## Request

### Headers

| Header        | Required | Description      |
| ------------- | -------- | ---------------- |
| Authorization | Yes      | Bearer API key   |
| Content-Type  | Yes      | application/json |

---

### Request Body

```json
{
  "email": "user@example.com",
  "context": "github_pr"
}
```

#### Request Fields

| Field   | Type   | Required | Description                    |
| ------- | ------ | -------- | ------------------------------ |
| email   | string | Yes      | Email address to verify        |
| context | string | No       | Usage context for AI decisions |

#### Supported Context Values

- signup
- github_pr
- form
- api

Context enables Trustmail AI to adapt trust logic based on usage.

---

## Response — Success (200 OK)

```json
{
  "email": "u***@example.com",
  "valid": true,
  "trust_score": 82,
  "risk_level": "low",
  "checks": {
    "syntax": true,
    "mx": true,
    "disposable": false,
    "role_based": false
  },
  "confidence": "high"
}
```

---

### Response Fields

| Field       | Description                     |
| ----------- | ------------------------------- |
| email       | Masked email for privacy        |
| valid       | Overall email validity          |
| trust_score | Trust score (0–100)             |
| risk_level  | Risk classification             |
| checks      | Explainable verification checks |
| confidence  | Human-readable confidence       |

---

### Validation Checks

| Check      | Description                |
| ---------- | -------------------------- |
| syntax     | Email format validation    |
| mx         | Mail server existence      |
| disposable | Disposable email detection |
| role_based | Role-based email detection |

---

## Risk Levels

| Trust Score | Risk Level |
| ----------- | ---------- |
| 80–100      | Low        |
| 50–79       | Medium     |
| 0–49        | High       |

---

## Error Responses

### Invalid Email

```json
{
  "error": true,
  "code": "INVALID_EMAIL",
  "message": "Email format is invalid"
}
```

---

### Unauthorized (401)

```json
{
  "error": true,
  "code": "UNAUTHORIZED",
  "message": "Invalid API key"
}
```

---

### Rate Limited

```json
{
  "error": true,
  "code": "RATE_LIMITED",
  "message": "Too many requests"
}
```

---

## HTTP Status Codes

| Status Code | Meaning               |
| ----------- | --------------------- |
| 200         | Success               |
| 400         | Bad request           |
| 401         | Unauthorized          |
| 429         | Rate limited          |
| 500         | Internal server error |

---

## Privacy & Security

- Emails are never returned in full
- No email data stored without consent
- Designed for privacy-compliant systems

---

## Versioning

This API follows semantic versioning.
Breaking changes will be released under a new major version.

---

## Roadmap

- Bulk email verification
- Webhook callbacks
- Context-aware AI tuning
- Expanded confidence explanations
