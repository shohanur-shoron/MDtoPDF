# Bangla-to-IPA Converter

`bangla_to_ipa` is a Python library dedicated to converting Bengali (Bangla) text into its International Phonetic Alphabet (IPA) representation. It aims to provide an accurate, rule-based phonetic transcription, considering individual characters, common diacritics, and crucially, Bengali consonant conjuncts (যুক্তাক্ষর).

This tool is useful for:
*   Linguistic research and phonetic analysis of Bengali.
*   Preprocessing text for Text-to-Speech (TTS) systems requiring phonetic input.
*   Educational purposes for learning Bengali pronunciation.
*   Developing other NLP tools that benefit from phonetic information.

## Features

*   **Character Mapping:** Converts individual Bengali vowels, consonants, and common diacritics (e.g., hasanta, anusvara, candrabindu) to their IPA equivalents.
*   **Conjunct Handling:** Utilizes an extensive mapping for common Bengali consonant conjuncts to ensure accurate phonetic representation, as conjunct pronunciation often deviates from the simple concatenation of individual consonant sounds.
*   **Longest Match Principle:** Prioritizes the longest possible match for conjuncts to accurately transcribe complex consonant clusters.
*   **Punctuation Removal:** Automatically removes common Bengali and English punctuation before conversion to produce clean phonetic output.
*   **Rule-Based:** Provides interpretable and consistent conversions based on defined linguistic rules and mappings.

## Installation

You can install `bangla_to_ipa` directly from PyPI using pip:

```bash
pip install bangla_to_ipa
```

Make sure you have Python 3.7 or higher installed.

## Usage

The primary way to use the library is through the `to_ipa()` function.

```python
from bangla_to_ipa import to_ipa

# Example 1: Simple word
bengali_text_1 = "বাংলা"
ipa_text_1 = to_ipa(bengali_text_1)
print(f"'{bengali_text_1}' -> IPA: {ipa_text_1}")
# Expected IPA (will depend on your internal mapping): 'baŋla' or 'baŋlɑ'

# Example 2: Word with a conjunct
bengali_text_2 = "অপেক্ষা" # ক্ষ (kṣa) is a common conjunct
ipa_text_2 = to_ipa(bengali_text_2)
print(f"'{bengali_text_2}' -> IPA: {ipa_text_2}")
# Expected IPA: 'ɔpekkʰa' or similar, accurately representing ক্ষ

# Example 3: Sentence with various elements
bengali_text_3 = "বিজ্ঞানীরা ঐক্যবদ্ধ হয়েছেন।" # জ্ঞ, ক্◌য, দ্◌ধ are conjuncts
ipa_text_3 = to_ipa(bengali_text_3)
print(f"'{bengali_text_3}' -> IPA: {ipa_text_3}")
# Expected IPA: 'biggæninera oikkobɔddʰoɦɔe̯tʃʰen' (approximation, actual output depends on your precise mappings)

# Example 4: Text with punctuation
bengali_text_4 = "হ্যালো, বিশ্ব!"
ipa_text_4 = to_ipa(bengali_text_4)
print(f"'{bengali_text_4}' -> IPA: {ipa_text_4}")
# Expected IPA for "হ্যালো বিশ্ব": 'ɦælo biʃʃo' (punctuation removed)

# Example 5: Word with Anusvara and Candrabindu
bengali_text_5 = "চাঁদ এবং রং"
ipa_text_5 = to_ipa(bengali_text_5)
print(f"'{bengali_text_5}' -> IPA: {ipa_text_5}")
# Expected IPA: 'tʃɑ̃d ebɔŋ rɔŋ' (demonstrating nasalization)
```

## How It Works

The `bangla_to_ipa` converter follows these main steps:

1.  **Punctuation Removal:** The input Bengali text is first cleaned by removing a predefined set of common Bengali and English punctuation marks (e.g., `।`, `!`, `,`, `.`, `?`, etc.). This ensures that only linguistic content is processed for phonetic conversion.

2.  **Iterative Longest Match:** The core conversion logic iterates through the cleaned sentence. At each position, it attempts to find the longest possible sequence of Bengali characters (starting from that position) that matches an entry in its internal consonant conjunct mapping.
    *   If a multi-character conjunct is found (e.g., "ক্ষ", "জ্ঞ", "ক্ত"), its corresponding predefined IPA string is appended to the output. The processing cursor then advances past this matched conjunct.
    *   If no multi-character conjunct is matched at the current position, the system falls back to matching a single Bengali character (vowel, consonant, or diacritic) against its individual character-to-IPA map. Its IPA equivalent is then appended, and the cursor advances by one character.

3.  **Character & Conjunct Mappings:** The accuracy of the conversion heavily relies on two internal data structures (likely within a `conversion_data.py` or similar module internal to the library):
    *   **`bangla_char_to_ipa` (or similar):** A dictionary mapping individual Bengali characters (vowels like 'অ', 'আ', 'ই'; consonants like 'ক', 'খ', 'গ'; and diacritics like '্', 'ং', 'ঁ') to their respective IPA symbols.
    *   **`bangla_conjunct_to_ipa` (or similar):** A dictionary mapping common Bengali consonant conjuncts (e.g., 'ক্ষ': 'kkʰɔ', 'জ্ঞ': 'ggɔ̃', 'ত্থ': 't̪t̪ʰɔ') to their specific IPA representations. This is crucial as conjuncts often have unique phonetic realizations. The keys in this dictionary should be ordered or processed to ensure the longest match principle is effective (e.g., by trying to match longer conjuncts before shorter ones if there's overlap, or by iterating from longest possible substring to shortest).

The iterative process continues until the entire input string has been processed.

## Linguistic Data (`conversion_data.py`)

The heart of `bangla_to_ipa` lies in its linguistic data, typically stored in internal mapping dictionaries. The quality and comprehensiveness of these mappings directly determine the converter's accuracy.

*   **Individual Character Map:** Contains mappings for:
    *   Independent Vowels (অ, আ, ই, ঈ, উ, ঊ, ঋ, এ, ঐ, ও, ঔ)
    *   Consonants (ক থেকে চন্দ্রবিন্দু পর্যন্ত)
    *   Vowel Diacritics (কার-চিহ্ন: া, ি, ী, ু, ূ, ৃ, ে, ৈ, ো, ৌ)
    *   Other Diacritics (ং, ঃ, ঁ, ্)
*   **Consonant Conjunct Map (যুক্তাক্ষর):** This is the most critical part for accurate Bengali IPA. It should include a wide range of common and even some less common conjuncts with their specific phonetic transcriptions.
    *   Examples: `ক্ষ` (ক+ষ), `জ্ঞ` (জ+ঞ), `ঞ্চ` (ঞ+চ), `ঙ্ক` (ঙ+ক), `ক্ত` (ক+ত), `হ্ম` (হ+ম), `ত্ত্ব` (ত+ত+ব-ফলা), ` স্ক্র` (স+ক+র-ফলা).

The creation and curation of these mappings require careful linguistic consideration.

## Limitations

*   **Rule-Based Nature:** Being rule-based, the converter's accuracy is entirely dependent on the completeness and correctness of its internal mapping tables. It may not correctly handle:
    *   Extremely rare or archaic conjuncts not present in its data.
    *   Context-dependent phonetic variations not explicitly encoded in the rules (e.g., schwa deletion nuances, allophonic variations).
    *   Loanwords from other languages that have non-standard Bengali phonetic adaptations unless they are specifically added to a custom mapping.
*   **Dialectal Variation:** The current version likely targets a standard Bengali pronunciation (e.g., Standard Colloquial Bengali - চলিত ভাষা). It does not account for regional dialectal variations in pronunciation.
*   **Prosody and Intonation:** This tool focuses on segmental phonetics (individual sounds) and does not attempt to represent prosodic features like stress, tone (if applicable), or intonation.

## Contributing

Contributions are highly welcome! If you find a bug, have suggestions for improving the mappings, or want to add more conjuncts:

1.  Please check the [Issue Tracker](https://github.com/shohanur-shoron/bangla_normalizer/issues) to see if the issue has already been reported.
2.  If not, open a new issue describing the problem, providing examples of incorrect conversion, or suggesting new mappings.
3.  For code or mapping contributions, please fork the repository, create a new branch for your changes, and submit a pull request. Ensure that any new mappings are linguistically sound.

## Future Scope

*   Expanding the conjunct and character mappings for even greater coverage.
*   Investigating options for handling common loanwords with specific phonetic adaptations.
*   Potentially exploring rules for more nuanced phonetic phenomena (e.g., basic schwa deletion).
*   Adding options for different transcription schemes or levels of phonetic detail.

## License

This project is licensed under the [MIT License] - see the `LICENSE` file for details.
