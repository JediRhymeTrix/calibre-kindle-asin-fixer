# ASIN Fixer for Kindle Colorsoft Cover Issues

This tool is designed to fix cover display issues on the new Kindle Colorsoft by ensuring the correct Kindle ASIN is present in `.opf` files within your Calibre library. Since the Colorsoft model fetches covers directly from Amazon servers, the correct Kindle ASIN is necessary for covers to load properly.

The tool performs the following actions:

1. **Extracts ASINs**: Reads `.opf` files in your Calibre library to pull existing ASINs.
2. **Scrapes Amazon**: Uses Selenium to scrape Amazon for the correct Kindle ASIN, if needed.
3. **Updates Metadata**: Writes the Kindle ASIN back into the `.opf` files to ensure the correct cover can be fetched.
4. **Updates Database**: Modifies entries in the Calibre database to store the updated ASIN information.

## Requirements

- **Python 3.x**
- **Calibre** with `.opf` metadata files for your books
- **Selenium** and **webdriver-manager** to handle Amazon scraping
- **SQLite3** for interacting with the Calibre database
- **ChromeDriver** for Selenium scraping (automatically managed by `webdriver-manager`)

## Installation

1. Clone this repository.
2. Install dependencies using the `requirements.txt` file:
   ```bash
   pip install -r requirements.txt
   ```
3. Ensure Chrome is installed (or modify the code to use a different browser if preferred).

## How It Works

The tool works by executing various subcommands. Below are details on each subcommand and its purpose.

### Subcommands

- **Extract ASINs**: Scans `.opf` files in your Calibre library to pull ASIN identifiers.
    ```bash
    python asin_fixer.py extract <root_dir> <output_file>
    ```
    - **`root_dir`**: Directory to start scanning (default is the current directory).
    - **`output_file`**: File to save the extracted ASINs (default is `amazon_ids.txt`).

- **Scrape Amazon for Kindle ASINs**: Looks up Kindle ASINs on Amazon and updates the `input_file`.
    ```bash
    python asin_fixer.py scrape <input_file>
    ```
    - **`input_file`**: File containing old ASINs and paths, as created by the `extract` command.

- **Update `.opf` Files**: Writes the newly found Kindle ASINs back into the `.opf` files.
    ```bash
    python asin_fixer.py update <mapping_file>
    ```
    - **`mapping_file`**: File containing old and new ASINs, as created by the `scrape` command.

- **Update Database**: Updates the Calibre database with the new ASINs from `.opf` files.
    ```bash
    python asin_fixer.py update_db <db_file>
    ```
    - **`db_file`**: Path to your Calibre database file.

- **Clean Temporary Data**: Removes lines with new ASINs and trailing commas from the input file.
    ```bash
    python asin_fixer.py clean <input_file>
    ```
    - **`input_file`**: File containing ASIN mappings, typically created during extraction and scraping.

## Example Workflow

1. **Extract ASINs from .opf files**:
   ```bash
   python asin_fixer.py extract /path/to/calibre/library amazon_ids.txt
   ```

2. **Scrape Amazon for updated Kindle ASINs**:
   ```bash
   python asin_fixer.py scrape amazon_ids.txt
   ```

3. **Update .opf files with new ASINs**:
   ```bash
   python asin_fixer.py update amazon_ids.txt
   ```

4. **Update the Calibre database with new ASINs**:
   ```bash
   python asin_fixer.py update_db /path/to/calibre/metadata.db
   ```

5. **Clean up temporary data if needed**:
   ```bash
   python asin_fixer.py clean amazon_ids.txt
   ```

## Notes

- This script uses **Selenium** for web scraping, so it will open a Chrome browser window (unless run in headless mode).
- **Captcha** may be required during Amazon scraping; the script will pause and wait for manual captcha resolution if detected.
- This tool is optimized for Kindle books with a corresponding ASIN on Amazon; results may vary if books don’t have Kindle variants.

## License

This project is licensed under the MIT License.

---
