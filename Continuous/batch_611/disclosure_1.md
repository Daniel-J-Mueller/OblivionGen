# 9026932

## Dynamic Content "Stack" Navigation

**Concept:** Expand the edge navigation concept beyond just books to *any* sequential digital content, and introduce a physical metaphor of a content “stack” with physics-based interaction.

**Specifications:**

**1. Content Types:**

*   Support for sequential content: eBooks, videos (playlists), audiobooks, image galleries, documents (PDFs, presentations), code files (scrolling through a script).
*   Content must be divisible into discrete “cards” or “layers” – representing pages, video clips, images, slides, code blocks, etc.

**2. Visual Representation – The Stack:**

*   Instead of a linear edge representation, visualize content as a physical stack of cards. The height of the stack corresponds to the total length of the content.
*   Each “card” represents a single unit of content (page, clip, image, etc.).
*   The stack is presented in a 3D-like perspective, allowing the user to see multiple layers at once.
*   The user's current location within the content is indicated by a highlighted card at the top of the stack.
*   Cards have subtle visual cues indicating content type (e.g., thumbnail for images, waveform for audio).

**3. Interaction – Physics-Based Navigation:**

*   **Horizontal Swipe (existing functionality):** Swiping horizontally reveals more of the stack.
*   **Vertical "Flick" (new):** A quick vertical flick *physically* rearranges the cards – shifting the current card up or down the stack. The speed of the flick dictates how many cards are skipped. This simulates physically flipping through the content.
*   **"Spread" Gesture:** A two-finger spread gesture "fans" out the cards, revealing more layers at once. The further the spread, the more cards are visible. Releasing the gesture causes the cards to smoothly re-stack.
*   **"Pull" Gesture:** Pulling down on the top card of the stack "peeks" at the next card in the sequence. Releasing slowly causes the stack to settle; releasing quickly flips to the next card.
*   **Card "Weight":** Assign a "weight" to each card based on content length or complexity. Heavier cards feel more substantial during interaction and have greater inertia.
*   **Haptic Feedback:** Haptic feedback simulates the feel of interacting with physical cards – the snap of flipping, the resistance of dragging, the texture of the card surface.

**4.  Dynamic Bookmarks & Metadata:**

*   Bookmarks are visualized as physical “tabs” protruding from the edge of the cards.  The tab length corresponds to the bookmark's relevance or user-defined importance.
*   Metadata (chapter titles, section headings, video timestamps) is displayed as labels on the card edges.
*   Users can customize the visual appearance of the stack – card colors, textures, label styles, and bookmark tab designs.

**5. Pseudocode (Vertical Flick Navigation):**

```
function onVerticalFlickDetected(velocityY) {
  // Calculate the number of cards to skip based on the velocity.
  cardsToSkip = abs(velocityY) * sensitivityFactor;

  //Limit cardsToSkip to the stack length
  cardsToSkip = min(cardsToSkip, stackLength);

  // Update current card index based on direction of flick
  if (velocityY > 0) { // Flick Up (Go Back)
    currentCardIndex = max(0, currentCardIndex - cardsToSkip);
  } else { // Flick Down (Go Forward)
    currentCardIndex = min(stackLength - 1, currentCardIndex + cardsToSkip);
  }

  // Animate transition to new card.
  animateCardTransition(currentCardIndex);

  // Provide haptic feedback.
  playHapticFeedback("flick");
}
```

**6. Hardware Considerations:**

*   High-refresh-rate display for smooth animations.
*   Precise touch sensor for accurate gesture recognition.
*   Advanced haptic engine for realistic feedback.
*   Powerful processor and GPU for rendering complex visuals.