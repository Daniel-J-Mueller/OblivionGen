# 10796687

**Selective Memory Persistence Profiles**

**Specification:**

This system expands upon the core 'delete first voice input' functionality by introducing user-defined persistence profiles that govern how long voice data is retained *before* the deletion command is even issued.  Instead of immediate deletion triggered by a second voice input, the system defaults to temporary storage governed by a user-selected profile.

**Components:**

*   **Profile Manager:** A module within the service provider environment responsible for creating, storing, and managing persistence profiles.
*   **Profile Types:** Predefined profiles (e.g., "Short-Term," "Meeting Notes," "Personal Diary," "Always Retain") and user-customizable profiles. Each profile defines a retention period (in minutes, hours, days, or indefinite) and associated actions upon expiry/deletion.
*   **Retention Engine:** A component that monitors the age of voice input representations and enforces the retention policies defined by the active profile.
*   **Contextual Profile Selection:** Mechanism to automatically select a profile based on detected context (e.g., calendar event type – “Meeting” triggers “Meeting Notes” profile; device location – “Home” defaults to “Personal Diary”).
*   **Override Mechanism:**  Allows the user to explicitly select a profile *before* providing voice input. (e.g., "Record this as a temporary note").

**Data Structures:**

*   `Profile`:  {`profile_id`: string, `name`: string, `retention_period`: duration, `actions_on_expiry`: [action_type], `default_expiry_action`: action_type}
*   `VoiceInputRecord`: {`voice_input_id`: string, `user_id`: string, `timestamp`: datetime, `data_location`: string, `profile_id`: string, `expiry_timestamp`: datetime}

**Workflow:**

1.  User provides voice input.
2.  System determines active profile (contextual selection or user override).
3.  A `VoiceInputRecord` is created, associated with the active profile, and data is stored. The `expiry_timestamp` is calculated.
4.  Retention Engine periodically checks `VoiceInputRecord` entries against the current time.
5.  If `expiry_timestamp` has passed:
    *   The `actions_on_expiry` are executed (e.g., automatic deletion, archival, transcription to text and storage, sending a summary).
6.  If a second voice input is received to “delete” the first:
    *   Deletion is immediate, *unless* the current profile overrides this (e.g., “Always Retain” ignores the delete command).
7.  User can query/modify profiles via voice command or application interface.

**Pseudocode (Retention Engine):**

```
function checkExpiry(voiceInputRecordList):
  for each record in voiceInputRecordList:
    if currentTime > record.expiry_timestamp:
      for each action in record.actions_on_expiry:
        executeAction(action, record)
      deleteRecord(record)
```

**Potential Actions:**

*   `DELETE`:  Delete the voice data.
*   `ARCHIVE`: Move to long-term storage.
*   `TRANSCRIPTION`:  Transcribe to text and store text.
*   `SUMMARY`:  Generate a brief summary of the voice data and store the summary.
*   `SEND_NOTIFICATION`: Notify the user about impending deletion/archival.
*    `SKIP_DELETION`: Ignore deletion instructions.