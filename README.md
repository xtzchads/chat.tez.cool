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
     - `🕵️ Stealth` (`0x02` classical / `0x04` PQ-Hybrid, ~0.001 XTZ, 0 storage burn): Encrypts both `{ to, text }`, masks the recipient's `X25519` public key with a one-time `SHA-256` pad, derives the 16-bit view tag from the shared secret (`X25519` in `0x02`, hybrid `X25519 + ML-KEM-512` in `0x04`), and self-mints to the sender (`to_ = sender`) so outsiders on-chain cannot cryptographically link the sender to the recipient
   - **Stealth Mode & Traffic Analysis (Timing Correlation)**: While Stealth mode (`0x02` / `0x04`) cryptographically hides the recipient's identity on-chain (`to_ = sender`), the blockchain still publicly records the sender's address (`op.sender`) and block timestamp (`op.timestamp`) of every transaction. If two accounts repeatedly send stealth messages back and forth in rapid succession while overall contract traffic is low, an external observer could infer a potential conversation link through turn-taking / timing correlation.
   - **Trust Model & Key Resolution**: To keep key registrations and encrypted payloads at 0 storage burn, `pk`, `pq_pk`, and `enc` are read from applied contract call history via the TzKT API (`api.tzkt.io`) and `https://tcinfra.net/rpc/tezos/mainnet`. Trusting the data returned by them is acceptable - while infrastructure owners can in theory perform some MiTM/spoofing and deanonymization attacks (but not decrypt existing messages), we tried to mitigate some of them - and additionally deem it very unlikely, so for now we accept this risk. To achieve zero risk of MiTM/deanonymization attacks, usage of your own rpc/indexer or communication client is recommended

