# Chad Chat

 - Chat with friends or troll the anons
 - Your message will be sent as NFT and will be seen in indexer and wallet by recipient
 - Perfect way to contact someone on-chain if there is no other way to do it
 - **Optional End-to-End Encryption (E2EE)**:
   - Uses **X25519 ECDH** key agreement + **AES-256-GCM** authenticated encryption
   - Deterministic X25519 keypair derived client-side via a domain-separated Tezos wallet signature (`requestSignPayload`)
   - 100% on-chain public key discovery via `KT1NuwjJ2wwaRGCL2yR3WeUzdELNzejVBJz7` token metadata (`pk` and `enc` fields) — zero off-chain key sharing required

