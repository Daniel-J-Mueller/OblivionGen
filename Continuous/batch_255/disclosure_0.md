# 11709925

## Dynamic Visual Password Generation via Generative Adversarial Networks

**Concept:** Leverage Generative Adversarial Networks (GANs) to dynamically create visual passwords tailored to individual user visual perception and evolving complexity levels. The system moves beyond static image selection to procedurally generate unique, personalized "passwords" on demand.

**System Specs:**

*   **Core:** GAN architecture consisting of a Generator and a Discriminator.
*   **Input (Generator):** Seed value (unique per user/session), complexity level (determined by security policy/user preference), and a feature vector representing the user’s visual preferences (captured during initial onboarding – see User Preference Capture below).
*   **Output (Generator):** A high-resolution image representing the visual password.
*   **Discriminator:** Trained to distinguish between real images (from a large training dataset) and generated images.  Crucially, the Discriminator *also* incorporates a “plausibility” metric – assessing whether the generated image *could* be a naturally occurring scene or object (to prevent trivially distinguishable patterns).
*   **Image Feature Extraction:** A pre-trained convolutional neural network (CNN) used to extract feature vectors from both real and generated images. These vectors are used for similarity comparisons (see Authentication Process).
*   **User Preference Capture:** During initial onboarding, the user is presented with a series of images with varying characteristics (color palettes, textures, object arrangements, complexity). The user’s selections and dwell times are recorded to build a vector representing their visual preferences. This vector is updated over time based on ongoing interactions.

**Authentication Process:**

1.  **Password Generation:** When the user attempts to authenticate, the system generates a visual password using the GAN, seeded with the user's unique ID, the current complexity level, and their visual preference vector.
2.  **Distraction Set Generation:**  A set of "distractor" images is also generated (or selected from a database). These images share *some* features with the generated password (e.g., similar colors, textures, object categories) but are *not* the correct password. The GAN is used to ensure these distractors are sufficiently similar to create a challenging authentication task.
3.  **Presentation:** The generated password and distractor images are presented to the user in a randomized order.
4.  **User Selection:** The user selects the image that represents the generated password.
5.  **Verification:** The selected image’s feature vector (extracted using the CNN) is compared to the expected feature vector of the generated password. A similarity threshold determines successful authentication.  The threshold can be dynamically adjusted based on the complexity level and user history.

**Pseudocode (Password Generation):**

```
function generate_visual_password(user_id, complexity_level, preference_vector):
    seed = hash(user_id + current_timestamp)
    noise = random_noise(seed)
    password_image = generator_network(noise, complexity_level, preference_vector)
    return password_image

function generate_distractor_images(password_image, num_distractors):
    distractors = []
    for i in range(num_distractors):
        noise = random_noise(seed=i)
        distractor_image = generator_network(noise, complexity_level, preference_vector) #Similar parameters
        distractors.append(distractor_image)
    return distractors
```

**Novelty & Potential:**

This system moves beyond static image selection to dynamically generated visual passwords, making them significantly more resistant to memorization and shoulder surfing attacks.  Personalization via user preference vectors increases usability and reduces cognitive load.  The GAN-based approach allows for fine-grained control over password complexity and visual characteristics.