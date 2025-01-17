<<CDSF/ENCRYPT>>
λ:2.0~τ:1705395200
∆{
  encryption:AES256-CBC,
  kdf:PBKDF2,
  iterations:100000,
  salt:random,
  iv:random,
  integrity:HMAC-SHA256
}
<<CDSF/ENCRYPT>>

[ENCRYPTED_CONTENT]
U2FsdGVkX19IjXtVv4RZHsK/n8K9x5yT2Q==
[Begin encrypted state block]
9f8uX0hLZ+zQY7vqJ2c5N8B4M6R1xWpT9kLmV3K8h/w=
eX7yP9nM4vF5tB2cR8jK3wL6xD0bN9mQ1sA5gH7vU+E=
... [encrypted state data continues] ...
kR3wS8pL2mN9vB4xC7tY5nF6gU8dQ1aZ0hJ3mK9cE+w=
[End encrypted state block]
[/ENCRYPTED_CONTENT]

<<CDSF/VERIFY>>
Integrity: SHA256=a1b2c3d4e5f6g7h8i9j0...
Timestamp: 1705395200
Version: 2.0
<<CDSF/VERIFY>>

The state has been encrypted with:
- Password-based key derivation (PBKDF2)
- AES-256-CBC encryption
- Random IV and salt
- HMAC-SHA256 for integrity verification

To decrypt this state file, you'll need:
1. The password ("my voice is my password")
2. The CDSF protocol version (2.0)
3. The encryption parameters specified in the header
