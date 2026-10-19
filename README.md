# Chad Chat

 - Chat with friends or troll the anons
 - Your message will be sent as NFT and will be seen in indexer and wallet by recipient
 - Perfect way to contact someone on-chain if there is no other way to do it
 - **Optional End-to-End Encryption (E2EE), Stealth Mode & Post-Quantum Shield**:
   - Uses **X25519 ECDH** key agreement + optional **NIST FIPS 203 ML-KEM-512** Post-Quantum KEM + **AES-256-GCM** authenticated encryption
   - Deterministic `X25519` + `ML-KEM-512` keypairs derived client-side from a single domain-separated Tezos wallet signature (`requestSignPayload`) — full Post-Quantum key-derivation resistance against Shor's algorithm applies to `tz1` (Ed25519, via the unexposed SHA-512 nonce prefix) and `tz5` (ML-DSA-44) accounts
   - 100% on-chain public key discovery & message indexing via **`KT1DenRaZdNgm1WM4vREJ2Zgx5EiH3aogDYZ`** (`pk`, `pq_pk`, and `enc` fields) — zero off-chain key sharing required
   - **3-mode privacy toggle (`🌐 Public` → `🔒 Encrypted` → `🕵️ Stealth`) + optional `🛡️ PQ-512` shield**:
     - `📡 Publish Key` / `🛡️ Publish PQ Key` (~0.001 XTZ, 0 storage burn): Broadcasts your `X25519` (32B) and optional `ML-KEM-512` (800B) public keys on-chain
     - `🔒 Encrypted` (`0x01` classical / `0x03` PQ-Hybrid, ~0.084 XTZ): Encrypts message content while minting the NFT directly to the recipient's wallet (`enc` and `pk` are stripped from `big_map` storage so ciphertexts cost 0 storage burn)
     - `🕵️ Stealth` (`0x02` classical / `0x04` PQ-Hybrid, ~0.001 XTZ, 0 storage burn): Encrypts both `{ to, text }`, masks the recipient's `X25519` public key with a one-time `SHA-256` pad, and self-mints to the sender (`to_ = sender`) so outsiders on-chain cannot see whom the message is directed to
   - **Trust Model & Key Resolution**: To keep key registrations and encrypted payloads at 0 storage burn, `pk`, `pq_pk`, and `enc` are read from applied contract call history via the TzKT API (`api.tzkt.io`) rather than persistent contract storage, and peer keys automatically resolve to the latest valid key published by each address without manual key pinning.

