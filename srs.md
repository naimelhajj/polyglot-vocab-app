## Vocabulary App — Feature & User-Flow Specification (AI-assisted, Dictionary-Grounded)

### Guiding Principles

1. **AI is allowed, but only as a coordinator**

* AI may be used to:

  * choose the best dictionary sources to consult
  * organize results into clean “meaning boxes”
  * merge duplicates and align senses across sources
  * translate **definitions** into the user’s chosen definition-display language
* AI must **never invent** meanings, parts of speech, examples, or translations. Every item shown must be grounded in dictionary sources.

2. **Source-exact dictionary content**

* Definitions, sense lists, part-of-speech labels, and example sentences are displayed **verbatim as retrieved from the chosen sources**.

3. **Supported languages (initial)**

* English, Arabic, French, Italian, Spanish.

---

## Profiles (One per language)

* The app has **exactly one profile per language**, created automatically by default:

  * English profile
  * Arabic profile
  * French profile
  * Italian profile
  * Spanish profile

Each profile is an independent learning space with its own:

* saved vocabulary
* review progress
* redundancy choices
* settings

---

## Per-Profile Settings

Each language profile has these editable settings:

### A) Definition display language

* The user chooses the language in which they want to **read definitions** inside that profile.
* Examples:

  * In the Arabic profile, the user may want definitions displayed in Arabic.
  * In the Italian profile, the user may want definitions displayed in English.

Rule:

* The app always retrieves and stores the **original dictionary definition text** (in the source’s original language).
* If the chosen definition display language differs, the app translates the definitions to that language and stores the translated version too.

### B) Translation target languages (for the headword only)

* The user chooses which languages they want to see as **translations of the word itself**.
* Important rule:

  * The translations shown in meaning boxes are **translations of the word (sense-aware)**, not translations of the definition text and not translations of example sentences.

---

## Lookup Experience

### Meaning boxes (sense-by-sense)

When looking up a word, results are shown as a scrollable list of **meaning boxes**, one per meaning.

Each meaning box contains:

* the word
* part of speech
* the definition (verbatim)
* one or more example sentences (verbatim)
* word-only translations for each selected translation target language (sense-aware)

### Sense-aware word translations

* Because a single word can have multiple meanings, translations are attached to the correct meaning box.
* The app uses the source’s sense structure (and example sentence when available) to keep translations aligned to the correct meaning.

### Redundant meanings

* The user can flag a meaning box as **redundant**.
* Redundant meaning boxes stop appearing everywhere (future lookups, reviews, games).

---

## Saving Vocabulary

* The user can save a word into the profile library.
* Saving preserves:

  * all non-redundant meaning boxes
  * definitions/examples
  * the word-only translations attached to each meaning
  * dictionary source attribution (what source(s) each meaning came from)

---

## Cross-Profile Capture (“Add this word to the target language profile”)

* When a meaning box shows a word translation (e.g., an Arabic or French equivalent), the user can tap it.
* The app offers: **“Add this word to the [target language] profile?”**
* If confirmed:

  * the app performs a normal lookup for that word using the **target profile’s preferred sources**
  * then automatically saves that word into the target profile’s library

This behaves exactly like a normal lookup, except it ends with an auto-save into that target profile.

---

## Caching (Critical)

The app caches aggressively so it stays fast and remains useful offline.

### What is cached

For each looked-up word (and for each meaning):

1. **Original dictionary content**

* the verbatim definition(s)
* part of speech
* example sentence(s)
* sense structure and ordering

2. **Word-only translations** (per profile’s chosen target languages)

* stored per meaning box

3. **Translated definitions** (only when the profile’s definition display language requires it)

* stored per meaning box

### Cache behavior when settings change

The cache must be adaptable and non-destructive:

* If the user changes translation target languages, the app can add new word-only translations while keeping older cached translations (even if they’re no longer currently selected).
* If the user changes the definition display language, the app:

  * keeps the original definitions
  * translates definitions into the new display language
  * caches those translated definitions too
  * continues to show definitions in the newly selected display language even for entries that were previously cached
* The cache can retain previously generated translated-definition variants (e.g., Arabic→English and Arabic→French) so switching back is instant.

### Offline behavior

* Previously looked-up entries remain viewable and learnable offline.
* If the user looks up a brand-new word while fully offline, the app should show a clear “not available offline yet” state.

---

## Reviews (Flashcards)

### Meanings are learned separately

* Each meaning box is treated as its own learnable unit.

### Two flashcards per meaning (required)

For every meaning box, generate exactly two cards:

1. **Meaning ➔ Word (production)**

* Front: definition + example sentence (example sentence shown smaller)
* Back: the word

2. **Word + example ➔ Meaning (recognition)**

* Front: the word + example sentence (example sentence shown smaller)
* Back: the meaning (definition + part of speech) + the word-only translations

### Scheduling algorithm

* Use an adaptive spaced repetition system **in the spirit of Cerego**:

  * performance-based scheduling
  * increasing intervals when the user answers correctly with confidence
  * decreasing intervals / faster reappearance when the user struggles
  * separate scheduling per meaning (not per word as a whole)

---

## Games (Practice beyond flashcards)

Optional mini-games reinforce:

* word ↔ meaning
* word ↔ translation
* usage through example sentences

Examples:

* Match tiles (word↔meaning, word↔translation)
* Multiple-choice drills (meaning, translation)
* Sentence builder (reconstruct the example sentence)
* Crossword-style practice (definitions/examples as clues)

Games must respect redundancy flags and prioritize meanings the user hasn’t mastered yet.

---

## Acceptance Criteria (High level)

* One default profile exists for each of: English, Arabic, French, Italian, Spanish.
* Each profile lets the user choose:

  * definition display language
  * translation target languages for the word-only translations
* Lookup shows meaning boxes with verbatim definitions, part of speech, examples, and sense-aware **word translations only**.
* Tapping a translation can add that word to the target language profile via normal lookup + auto-save.
* All looked-up entries are cached, including:

  * original dictionary content
  * word-only translations
  * translated definitions (when needed)
* Cache remains useful across settings changes and retains older cached variants.
* Reviews generate two flashcards per meaning and schedule them with a Cerego-like adaptive spaced repetition approach.
* The app remains useful offline for previously looked-up entries.
