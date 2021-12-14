# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.0.1] 2021-12-14
### Added
- `PrivateKey` and `PublicKey` types, offering the following functions:
```go
GenerateKey(rnd io.Reader) (sk *PrivateKey, err error) // to generate a new keypair
NewPrivateKey(skBytes []byte) (sk *PrivateKey, err error) // To deserialise a private key
(sk *PrivateKey) Public() PublicKey // To export a public key from a private
(sk *PrivateKey) Bytes() // To serialise a private key

NewPublicKey(pkBytes []byte) (*PublicKey, error) // To deserialise a public key
(sk *PublicKey) Bytes() // To serialise a public key

(sk *PrivateKey) Prove(alpha []byte) (beta, proof []byte, err error) // To generate proof and VRF
(pk *PublicKey) Verify(alpha, proof []byte) (verified bool, beta []byte, err error) // To verify proofs and VRFs
```