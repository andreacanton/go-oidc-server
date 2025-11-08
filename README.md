# go-oidc-server

> A minimalist **OpenID Connect (OIDC) Authorization Server** built from scratch in Go, using **the standard library exclusively**. Created for educational purposes.

This project is a "from-scratch" implementation of an OIDC server, built upon the secure foundations of **OAuth 2.1 Best Practices**. Its primary goal is didactic: to explore the core identity and authorization protocols using only the packages included in the standard Go installation.

This server serves as a comparative exercise to [a similar didactic OIDC server written in Bun.js](https://github.com/andreacanton/oicd-server), aiming to highlight the capabilities and challenges of a standard-library-only approach in Go.

## Philosophy and Challenges

* **Zero External Dependencies:** The `go.mod` file is (and will intentionally remain) empty. Everything is built using `net/http`, `crypto/rand`, `encoding/json`, etc.
* **Didactic and Transparent:** The code is written to be easily read and studied. The goal is to understand *how it works*, not to achieve maximum performance.
* **Focus on OAuth 2.1:** The server's core follows modern security standards, adhering to OAuth 2.1 (mandatory PKCE, omission of the Implicit Grant).
* **The "Standard-Lib" OIDC Challenge:** The most complex part of OIDC is the issuance of `id_token`s in **JWT** format. This project will tackle the creation, signing, and validation of JWTs using *only* standard packages (like `crypto/*` and `encoding/json`), avoiding external JWT libraries.

## Features and Roadmap

The server is implementing the OIDC and OAuth 2.1 specifications in phases.

* [ ] **Core OAuth 2.1**
    * [ ] Authorization Code Grant Flow
    * [ ] **PKCE (Proof Key for Code Exchange)** (RFC 7636) - *Mandatory* for all clients.
* [ ] **OIDC Discovery**
    * [ ] `GET /.well-known/openid-configuration` endpoint implemented.
* [ ] **Core OIDC (In Development)**
    * [ ] Handling of the `openid` scope.
    * [ ] Authorization endpoint (basic login/consent page).
    * [ ] Issuance of `id_token` (JWT) and `access_token`.
    * [ ] `userinfo` endpoint.
    * [ ] `jwks_uri` endpoint (for public signing keys).
* [ ] **Other Grants**
    * [ ] Client Credentials Grant
    * [ ] Refresh Token

## 🚀 Quick Start

**Prerequisites:**
* Go 1.18+ (or your chosen version)

```bash
# 1. Clone the repository
git clone [https://github.com/YOUR_USERNAME/go-oidc-server.git](https://github.com/YOUR_USERNAME/go-oidc-server.git)
cd go-oidc-server

# 2. Run the server
go run .

# The server is now listening on http://localhost:8080
```

## Persistence

To adhere to the "zero dependencies" philosophy, the persistence of clients, codes, and tokens is managed **in memory**.

Data (registered clients, user sessions, authorization codes) are saved in global maps protected by `sync.Mutex`. *Note: This is not suitable for production but is ideal for development and study.*

## API Endpoints

* `GET /.well-known/openid-configuration`: The OIDC Discovery endpoint, exposing server configuration metadata.
* `GET /authorize`: The OIDC Authorization Endpoint.
    * *Required parameters include:* `code_challenge` and `code_challenge_method`.
* `POST /token`: The Token Endpoint for exchanging codes/credentials for tokens.
    * *Required parameter:* `code_verifier`.
* `GET /userinfo`: Authenticated endpoint that give user informations from a token.

## 🤝 Contributing

This is an educational project, but contributions are welcome! If you find a bug, want to improve the documentation, or implement a missing specification feature, please open an Issue or a Pull Request.

## 📜 License

This project is released under the **GNU GENERAL PUBLIC LICENSE Version 3** (GPL-3).

In short, this means you are free to run, study, share, and modify the software. However, any derivative or distributed software that includes parts of this code must also be released under the GPL-3 license.

See the `LICENSE` file for the full text of the license.
