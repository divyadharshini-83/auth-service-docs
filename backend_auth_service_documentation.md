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

### Security Implementation Details

- **Cryptographically Secure OTP**: Generated using `crypto.randomInt(100000, 1000000)` to prevent pseudorandom predictability.
- **OTP Hashing**: Never stored in plain text. Hashed with HMAC-SHA256 with an independent server-side pepper secret (`OTP_SECRET`).
- **Constant-Time Verification**: Verification utilizes `crypto.timingSafeEqual` to safeguard against timing attacks.
- **Single-Use Enforcement**: Once verified, `is_used` is set to `true`, preventing replay attacks.
- **Short Lifetime**: OTPs expire strictly after 5 minutes (`OTP_EXPIRY_MINUTES=5`).
- **Attempt Tracking & Lockout**: Maximum 5 attempts (`OTP_MAX_ATTEMPTS=5`). Reaching the limit permanently invalidates the challenge.
- **Resend Cooldown**: Enforces a 60-second cooldown (`OTP_COOLDOWN_SECONDS=60`) between OTP generations.
- **Password Hashing**: `bcryptjs` with 12 salt rounds.
- **HTTP-Only Cookies & Bearer Tokens**: Session tokens are issued via HTTP-only, SameSite Secure cookies, with support for `Authorization: Bearer <token>` for API clients.
- **Data Leak Prevention**: Passwords, hashes, OTPs, and internal stack traces are stripped and never returned to the frontend.
- **Rate Limiting**:
  - Global API: 100 requests / 15 minutes
  - Auth (Registration & Login): 15 requests / 15 minutes
  - OTP (Send & Verify): 10 requests / 15 minutes

---