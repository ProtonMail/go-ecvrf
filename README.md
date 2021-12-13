# Go-ECVRF
Go-ECVRF is a library that implements ECVRF-EDWARDS25519-SHA512-TAI,
a verifiable random function described in draft-irtf-cfrg-vrf-10.

By design this VRF is **not suitable to be used on secret input**,
as the try-and-increment procedure leaks information of the message
(alpha string), but it is constant time for different secret keys.

## Download/Install
### Vendored install
To use this library using [Go Modules](https://github.com/golang/go/wiki/Modules) just edit your
`go.mod` configuration to contain:
```gomod
require (
    ...
    github.com/ProtonMail/go-ecvrf latest
)
```

It can then be installed by running:
```sh
go mod vendor
```
Finally your software can include it in your software as follows:
```go
import "github.com/ProtonMail/go-ecvrf/ecvrf"
```

## Documentation
A full overview of the API can be found here:
https://godoc.org/gopkg.in/ProtonMail/go-ecvrf/ecvrf

In this document we provide some examples, more can be found in the tests.

## Examples
### Generating a new VRF key
Using `nil` as a source will automatically use randomness from `crypto/random`
```go
import "github.com/ProtonMail/go-ecvrf/ecvrf"

privateKey, err := ecvrf.GenerateKey(nil)
publicKey, err := privateKey.Public()

PrivateKeyBin := privateKey.Bytes()
PublicKeyBin := []bytes(publicKey)
```
The private key can be used to prove, the public key to verify.

### Proving
```go
import "github.com/ProtonMail/go-ecvrf/ecvrf"

privateKey, err = ecvrf.NewPrivateKey(PrivateKeyBin)
message := []byte("alice")
vrf, proof, err := privateKey.Prove(message)
if err != nil {
    // handle error
}

// vrf contains the VRF output (or hash)
// proof contains the proof for that message and private/public keypair
```

### Verifying
```go
import "github.com/ProtonMail/go-ecvrf/ecvrf"

publicKey, err = ecvrf.NewPublicKey(PublicKeyBin)
verified, verificationVRF, err := publicKey.Verify(alice, proof)
if err != nil {
    // handle error
}

if verified {
    // proof verification succeeded, use verificationVRF
} else {
    // verification of the proof failed
}
```