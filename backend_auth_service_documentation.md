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

---


## 3. Architecture & Design *(Optional)*
- **Component Structure:** Compliant with **Section 10** of the Technical Test Specification:

```
modest-nobel/
├── .env                          # Local environment variables
├── .env.example                  # Environment configuration template
├── .gitignore                    # Git ignore file
├── package.json                  # Dependencies and test scripts
├── README.md                     # Documentation and setup instructions
├── schema.sql                    # SQL DDL migrations (MySQL & PostgreSQL)
├── postman_collection.json       # Complete Postman test collection
├── server.js                     # Application entry point with DB connect & graceful shutdown
├── src/
│   ├── app.js                    # Express app configuration, security middleware & routes
│   ├── config/
│   │   └── database.js           # Mongoose MongoDB connection & indexing
│   ├── controllers/
│   │   └── authController.js     # Request handlers for auth and protected pages
│   ├── middleware/
│   │   ├── authMiddleware.js     # JWT verification from Cookie / Bearer header
│   │   ├── errorHandler.js       # Centralized error handler (no leak of internals)
│   │   ├── rateLimit.js          # Rate limiters (auth, OTP, and global)
│   │   └── validation.js         # Server-side validation and sanitization
│   ├── models/
│   │   ├── OtpVerification.js    # OTP Verification Mongoose schema & indexes
│   │   └── User.js               # User Mongoose schema & virtuals
│   ├── routes/
│   │   ├── authRoutes.js         # Routes for /register, /login, /send-otp, /verify-otp, /logout, /me
│   │   └── userRoutes.js         # Routes for protected /connected page
│   ├── services/
│   │   ├── authService.js        # User registration, credential checks, session issuing
│   │   └── otpService.js         # Cryptographic OTP generation, hashing, attempt limits, cooldown
│   └── utils/
│       ├── apiResponse.js        # Standardized API response format helper
│       ├── otp.js                # Cryptographic random OTP generator & constant-time hash verify
│       └── password.js           # bcrypt hashing and comparison
└── tests/
    └── auth.test.js              # Comprehensive Jest/Supertest suite covering Section 10 test cases
```
---