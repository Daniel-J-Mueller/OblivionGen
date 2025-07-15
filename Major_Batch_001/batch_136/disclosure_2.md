# 10102386

## Dynamic IV Watermarking

**Concept:** Extend the initialization vector (IV) manipulation to embed a dynamic, per-frame watermark imperceptible to standard decryption, requiring a separate, time-synchronized key delivery channel.

**Specifications:**

*   **IV Structure:** The IV will be structured as follows (assuming 128-bit IV, adjustable):
    *   Bits 64-71: Content Type (as in the original patent).
    *   Bits 72-87: Watermark Seed – a pseudo-random number generator (PRNG) seed.
    *   Bits 88-95: Frame Counter – increments per frame.
    *   Bits 96-127: Cryptographic Nonce & Counter (standard AES-CTR requirements)
*   **Watermark Generation:**
    *   On the encoding side, a PRNG is seeded with the `Watermark Seed` from the IV *and* a master watermark key (separate from the encryption key).
    *   The PRNG generates a pseudo-random value for each frame.
    *   This value is subtly XORed with the least significant bits of the encrypted media sample *before* transmission. This modification is within the noise floor and isn't detectable via standard analysis.
*   **Decryption & Verification:**
    *   During decryption, the `Watermark Seed` is extracted from the IV.
    *   A *second* key delivery channel (e.g., a secure timestamp server, physically secure communication) provides the master watermark key to the decoder.
    *   The decoder replicates the PRNG process using the `Watermark Seed` and master watermark key.
    *   The decoder then XORs the extracted PRNG value with the decrypted sample to reveal the embedded watermark.
    *   This watermark could be a frame-specific identifier, a content origin flag, or even a subtle modification to the image/audio quality.
*   **Frame Synchronization:** Precise frame synchronization between the encoder and decoder is critical. The `Frame Counter` in the IV helps maintain this synchronization.  Lost synchronization requires re-establishment of the watermark (a short, secure re-keying process).
*   **Security Considerations:** The security relies on the secrecy of the master watermark key *and* the secure delivery of that key. The `Frame Counter` protects against replay attacks. The subtle nature of the watermark makes detection without the master key very difficult.

**Pseudocode (Decoder Side):**

```
function decrypt_and_verify(encrypted_sample, IV, master_watermark_key):
    content_type = extract_content_type(IV)
    watermark_seed = extract_watermark_seed(IV)
    frame_counter = extract_frame_counter(IV)

    //Standard AES Decryption (using IV nonce and counter)
    decrypted_sample = aes_decrypt(encrypted_sample, IV)

    //Recreate PRNG using watermark seed and master key
    prng = initialize_prng(watermark_seed, master_watermark_key)
    prng_value = generate_random_value(prng)

    //Reveal watermark
    watermark = decrypted_sample XOR prng_value

    return watermark, decrypted_sample

```