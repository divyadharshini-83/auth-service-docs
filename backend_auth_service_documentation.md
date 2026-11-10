# [auth-service ]

## 1. Developer & Ownership Details
- **Developer Name:** Divyadharshini
- **Developer ID / Employee ID:** EMP1042
- **Role / Responsibility:** Backend Developer
- **Team Leader:** xxx
- **Last Updated:** November 2026

---

## 2. Overview & Purpose
- **Path:** `backend-auth-service`
- **Type:** Backend Service
- **Description:** This backend service implements a hardened, production-grade authentication flow. The server validates and sanitizes all input strictly server-side, independently of any frontend client.

  Sensitive credentials (passwords, OTPs) are never stored in plain text and are never exposed in API responses or logs. Authenticated sessions are established using HTTP-only, Secure cookies with SameSite policy protection, with full fallback support for `Authorization: Bearer <token>` headers for REST/Postman testing.