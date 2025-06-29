# Writwist: Intelligent Transliteration Metapackage

**Writwist** is envisioned as a powerful Python 3 metapackage designed to provide comprehensive transliteration services. It will intelligently convert text from one writing system (script) to another, understanding the nuances of languages and their orthographic conventions.

Think of Writwist as your future go-to toolkit when you need to:
*   Convert Hindi text written in Latin script to the Devanagari script.
*   Transliterate names or terms between English and Arabic, Tamil, Cyrillic, or dozens of other scripts.
*   Ensure that the output text is not just a character-by-character mapping but also respects script-specific rules for correct formation and rendering.

## Part 1: General Audience

### What is Writwist?

Writwist will be a "metapackage," meaning it bundles and orchestrates several specialized libraries to offer a unified and enhanced transliteration experience. Instead of you needing to find, install, and learn multiple tools for language detection, code handling, transliteration, and text shaping, Writwist will aim to provide a single, easy-to-use interface.

It will handle tasks like:
*   Identifying the language of your input text.
*   Converting text between a vast array of global scripts.
*   Applying complex orthographic rules specific to each language and script.
*   Preparing text for correct display by utilizing text shaping.

### Who is it for?

Writwist will be beneficial for:

*   **Developers:** Integrating transliteration into applications such as content management systems, social media platforms, search engines, or data processing pipelines.
*   **Linguists and Researchers:** Working with multilingual text data, standardizing script representations, or studying language variations.
*   **Content Creators & Publishers:** Making content accessible across different linguistic communities by converting scripts.
*   **Anyone needing to convert text between different writing systems** for personal or professional use.

### Why will it be useful?

Transliteration is more than just swapping characters. Different languages, even when using similar scripts, have unique rules. Moreover, many scripts (like those for Indic or Arabic languages) require "shaping" for characters to combine and display correctly.

Writwist will aim to simplify this complexity by:

*   **Accuracy:** Leveraging powerful underlying engines like Aksharamukha for high-quality transliteration that respects linguistic rules.
*   **Ease of Use:** Providing a straightforward command-line interface (CLI) and a Python API.
*   **Comprehensiveness:** Supporting a vast number of scripts and languages.
*   **Intelligence:** Offering features like automatic language detection.
*   **Correct Rendering:** Integrating text shaping capabilities for scripts that require it.
*   **All-in-One:** Reducing the need to manage multiple individual libraries for different aspects of the transliteration workflow.

### Planned Features

*   **Extensive Script Support:** Transliteration between a wide range of global writing systems, with a special focus on Indic scripts.
*   **Automatic Language Detection:** Identify the source language/script of the input text automatically.
*   **Standard Language Code Handling:** Robust parsing and management of IETF language tags (e.g., `en`, `hi-Deva`, `ar-Latn`).
*   **Orthographic Correctness:** Application of language-specific rules for vowel sounds, consonant clusters, and other orthographic conventions.
*   **Text Shaping Integration:** Output text that is correctly shaped for rendering, especially for complex scripts.
*   **Flexible CLI:** A user-friendly command-line tool for quick conversions.
*   **Python API:** A simple and powerful API for integration into other Python projects.
*   **Configuration for Customization:** (Potential) Options to fine-tune transliteration behavior.

### Installation

Once released, you will be able to install Writwist using pip:

```bash
pip install writwist
```

This will install Writwist along with all its necessary dependencies.

### Basic Usage (Future CLI)

Writwist will offer a command-line interface for quick transliteration tasks. The exact commands are yet to be finalized, but here's a conceptual example:

**Example: Transliterating a Hindi phrase from Latin to Devanagari:**

```bash
writwist "meraa naam jules hai" --from hi-Latn --to hi-Deva
```
*Output (expected):*
```
मेरा नाम जूल्स है
```

**Example: Auto-detecting source and transliterating to a target script:**
(Assuming the tool can detect the input is English)
```bash
writwist "Hello world" --to Tamil
```
*Output (conceptual):*
```
ஹலோ வர்ல்ட்
```

*(Note: The CLI (`writwist/__main__.py`) is planned and not yet implemented.)*

### Basic Usage (Future Programmatic API)

Writwist will also provide a Python API for more complex integrations.

**Conceptual Python Example:**

```python
from writwist import transliterate, detect_language

text_to_convert = "namaste duniya"

# Future: Auto-detect language
# source_lang_script = detect_language(text_to_convert)
# print(f"Detected language-script: {source_lang_script}") # Example: 'hi-Latn'

# Transliterate
# Assuming source is Hindi in Latin script, target is Devanagari
# transliterated_text = transliterate(text_to_convert, target_script="Deva", source_script="hi-Latn")
# print(transliterated_text)
# Expected: नमस्ते दुनिया

# Conceptual: Transliterate with shaping for display
# shaped_transliterated_text = transliterate(text_to_convert, target_script="Deva", source_script="hi-Latn", shape=True)
# This would return data suitable for rendering engines that understand HarfBuzz output.
```

*(Note: The programmatic API is planned and not yet implemented in `writwist/__init__.py` or other modules.)*

## Part 2: Technical Details

### How it Will Work

Writwist will function as an intelligent orchestrator for a suite of powerful open-source libraries:

1.  **Input & Language/Script Identification:**
    *   The user will provide input text and optionally specify the source and target language/script.
    *   If the source is not specified, Writwist will aim to use **`fasttext-langdetect`** or **`lumi-language-id`** to predict the language of the input text.
    *   Language and script tags (e.g., `en-US`, `hi-Deva`) will be parsed and normalized using **`langcodes`**, ensuring standard BCP 47 compliance.

2.  **Core Transliteration:**
    *   The primary transliteration engine will be **`aksharamukha`**. Aksharamukha is a versatile library that supports a vast number of scripts, especially those of the Indic cultural sphere. It handles complex orthographic rules, ensuring that the conversion is not merely character-to-character but also contextually and linguistically accurate.

3.  **Text Shaping (for Rendering):**
    *   For scripts that require complex glyph interactions for correct display (e.g., Indic scripts, Arabic), Writwist will utilize **`uharfbuzz`**. `uharfbuzz` provides Python bindings to the HarfBuzz text shaping engine. This step will convert the transliterated text into a sequence of correctly positioned glyphs, applying OpenType features like ligatures and kerning. This ensures the text is ready for accurate rendering in graphical environments.

4.  **Output:**
    *   The final output will be the transliterated text. If shaping is requested, the output might be more complex, representing glyph strings and their positions, suitable for rendering pipelines.

### Key Dependencies and Their Roles

*   **`aksharamukha` (>=1.8.1):** The core engine for script conversion and transliteration. Handles a multitude of scripts and their specific orthographic rules. (License: GNU AGPL 3.0)
*   **`langcodes` (>=3.1.0):** For robust handling, parsing, and standardization of IETF language tags (BCP 47). Essential for specifying source and target languages/scripts. (License: MIT)
*   **`fasttext-langdetect` (>=1.0.2):** A fast and accurate language identification library based on Facebook's fastText. Will be used for auto-detecting the input language. (License: MIT)
*   **`lumi-language-id` (>=0.2.1):** Another language identification library. Writwist may use this as an alternative or complement to `fasttext-langdetect`. (License: Typically MIT, based on other LuminosoInsight packages)
*   **`uharfbuzz` (>=0.16.1):** Python bindings for the HarfBuzz text shaping engine. Will be used to ensure correct glyph rendering for complex scripts. (License: Apache-2.0)
*   **`yaplon` (>=1.5.7):** A utility for converting between YAML, JSON, PLIST, XML, and CSV data formats. Its role in Writwist might be for internal configuration management or as a bundled utility for developers. (License: MIT)
*   **`lxml` (>=4.6.2):** A powerful and Pythonic library for processing XML and HTML. May be used if Writwist needs to handle transliteration rules from XML files or extract text from XML/HTML documents. (License: BSD)

### Codebase Structure (Current)

*   `writwist/`: Main package directory.
    *   `__init__.py`: Currently contains the version. Will house the main programmatic API.
    *   `__main__.py`: (Planned) Will contain the command-line interface logic.
*   `setup.py`: Standard Python package setup script, defines metadata and dependencies.
*   `requirements.txt`: Lists runtime dependencies.
*   `README.md`: This file.
*   `LICENSE`: Contains the Apache License 2.0 under which Writwist itself is distributed.
*   `dist.sh`: Shell script for building and distributing the package. *(Note: This script currently refers to "yaplon" as the APP_NAME, which should likely be "writwist").*

### Contributing

Writwist is currently in its early stages. Contributions will be welcome once the foundational CLI and API structures are in place. Future contribution guidelines will likely include:

*   **Reporting Issues:** Use the GitHub issue tracker for bugs and feature requests.
*   **Development Setup:**
    ```bash
    git clone https://github.com/twardoch/writwist.git
    cd writwist
    python3 -m venv venv
    source venv/bin/activate  # or venv\Scripts\activate on Windows
    pip install -e .[dev]
    ```
*   **Coding Conventions:** Adherence to PEP 8 Python style guidelines.
*   **Testing:** New features or fixes should ideally be accompanied by tests.
*   **Pull Requests:** Submit PRs against the main branch with clear descriptions of changes.

### License

Writwist is licensed under the **Apache License 2.0**. Please note that its dependencies are available under their own respective licenses (mentioned above).

---

*Author: Adam Twardoch ([adam+github@twardoch.com](mailto:adam+github@twardoch.com))*
*Project URL: [https://twardoch.github.io/writwist/](https://twardoch.github.io/writwist/)*
*Source: [https://github.com/twardoch/writwist/](https://github.com/twardoch/writwist/)*
