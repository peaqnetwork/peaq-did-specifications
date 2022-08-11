# PEAQ DID specifications
## Introduction
The peaq DID standard supports machines to discover each other, carry out transactions, verify claims, preserve privacy and  remain sovereign.
A core value proposition of peaq DID is the standardization of machine interaction through a consortium of diverse industry leaders in a self sovereign world by providing standard interfaces for discovery which allows two participants to successfully engage in a transaction.

## Peaq DID Format
The peaq DID method shall be identified by the method name peaq in lower case and follow the below given format
| |
|---|
|peaq-did = "did:peaq:" id-string|
|id-string  = 1* idchar|
|idchar   = 1-9 / A-H / J-N / P-Z / a-k / m-z |

## Status of the document
This document is currently in draft stage and is the first iteration of the peaq DID which provisions means to enable successful on-boarding and commercial operations related to vehicles and vehicle charging stations.
This is a foundation draft and will be refined to accommodate additional use cases in forthcoming iterations to make this DID standard more generic and robust by taking into account the learnt lessons.
In coming iterations to this document, references to charging stations will be generalized to reflect machines. 

## peaq DID Document Example
```
{
  "id": "did:peaq:5HRNr4pXH7PYKEmeW1jzJVxepXyg8w2Q3YpgRNHpH8foNr5i",
  "controller": "did:peaq:5HRNr4pXH7PYKEmeW1jzJVxepXyg8w2Q3YpgRNHpH8foNr5i",
  "verificationMethod": [
    {
      "id": "#pk1",
      "type": "Ed25519VerificationKey2020",
      "controller": "did:peaq:5HRNr4pXH7PYKEmeW1jzJVxepXyg8w2Q3YpgRNHpH8foNr5i",
      "publicKeyMultibase": "z5HRNr4pXH7PYKEmeW1jzJVxepXyg8w2Q3YpgRNHpH8foNr5i" 
    }
  ],
  "signature" : 
    {
       type: "Ed25519VerificationKey2020",
       issuer: "<did address of Peaq>",
       hash: "<signature>"
    },
  "service": [
    {
      "id": "#payment",
      "type": "payment",
      "serviceEndpoint": "5HRNr4pXH7PYKEmeW1jzJVxepXyg8w2Q3YpgRNHpH8foNr5i"
    },
    {
      "id": "#p2p",
      "type": "p2p",
      "serviceEndpoint" : "/ipv4/w.x.y.z/tcp/port/p2p-circuit/p2p/QmPublicKey"
    },
    {
      "id": "#metadata",
      "type": "metadata",
      "serviceEndpoint" : "/ipv4/w.x.y.z/tcp/port/p2p-circuit/p2p/QmPublicKey"
    }
  ],
  "authentication": [
    "#pk1"
  ]
}
```

Taking into account our base use case of charging stations, following are few key takeaways as specified in the example above
- Metadata regarding the machine/charging station can be retrieved from the service object with id and type set to metadata. The returned metadata is encoded using protobuf and bears the following structure.
- To request the charging station to start the charging process and in order to derive the multi-sign wallet address, the provided address under "serviceEndpoint" corresponding to the service of type "payment" and id "#payment" can be used. 
- There should be a p2p service present allowing others to communicate with this entity using libp2p networking. All further communications will be handled by the channels established by means of this service.
- All verifications must make use of the key specified by the authentication attribute of the DID document.

## peaq DID Security

This section provides the security consideration for the DID method implementation.

1) Firstly the peaq blockchain is based on the Substrate blockchain framework, and as such, it heavily relies on its implementation for most of the cryptographic operations. 
2) Secondly did:peaq is supported by ed25519 and sr25519 is immune to cache-timing attacks , hyperthreading attacks and other side-channel attacks. 
3) Thirdly since DID document are always to be signed and the 'chain of custody' of the signing key can be traced throughout the identity lifespan, replay, message insertion, deletion, modification and man-in-the-middle attacks are prevented. 
4) Implementation of best practices: The did:peaq implements security auditing, logging, and proactive monitoring according to industry best practices. The did:peaq utilizes best practices regarding a secure software supply chain for any libraries or other software components in use.

## peaq DID privacy 

This section provides the privacy consideration for the DID method implementation.

The peaq supports verifiable credentials that are issued to a specific DID, hence the same DID needs to be revealed upon a credential presentation and the DID subject must prove ownership of the DID being used.

Personal Identity Information :No personal identity information to be stored in a DID document if necessary the integrity and confidentiality of personal data must be ensured. If required, DID authority ensures that necessary security controls are in place to protect user personal data in addition this it be ensured that no personal data or personally identifying information is written on any system memory, or log at the system or application level. 

