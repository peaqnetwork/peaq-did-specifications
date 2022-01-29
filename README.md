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

