# Patchouli CLI

A command-line tool designed to help manga collectors integrate their local manga files with Calibre, leveraging metadata from [Bangumi (bgm.tv)](https://bgm.tv/).

## Prerequisites

*   Python 3.x
*   `requests` library (`pip install requests`)
*   `calibredb` command-line tool (part of a Calibre installation). Ensure it's accessible in your system's PATH.

## Installation

1.  Make the script executable:
    ```bash
    chmod +x patchouli
    ```

## Directory Naming Convention

For `patchouli` to accurately identify your manga series and fetch correct metadata, your series directories must follow one of these strict naming conventions:

*   **`Manga Name - Author Name [bgm_ID]`**: (Recommended) Provides the most precise match, using the Bangumi ID directly.
    *   Example: `よつばと! - あずまきよひこ [bgm_10086]`
*   **`Manga Name - Author Name`**: Searches Bangumi using both the manga name and author.
    *   Example: `進撃の巨人 - 諫山創`

Any directories not conforming to these patterns will be skipped.

## Volume Filename Handling

`patchouli` attempts to extract the volume number from the filenames within the series directory to correctly assign metadata and covers. The script looks for the following patterns in the filename (where `N` is the volume number):

*   `... - 卷N ...` (e.g., `Fire Punch - 卷01.epub`)
*   `... - Vol.N ...` (e.g., `Fire Punch - Vol.01.epub`)
*   `... - VolN ...` (e.g., `Fire Punch - Vol01.epub`)
*   `... - N ...` (e.g., `Fire Punch - 01.epub`)

If the volume number can be successfully guessed, the script will:
1.  Fetch specific metadata for that volume if available on Bangumi.
2.  Set the series index in Calibre to match the volume number.
3.  Download and set the cover image.
4.  Set the publication date (release date) based on Bangumi data.

**Important:** If the volume number cannot be determined from the filename, the script will **not** assign a series index and will **not** download a cover image for that file.

## Usage

### Add Manga to Calibre

The `add` command adds manga files to your Calibre library. You can process a single series directory or batch process a collection directory.

#### 1. Add a single series
```bash
./patchouli add <path_to_series_directory> -l <lang_code> [options]
```

#### 2. Add a collection (Batch mode)
```bash
./patchouli add -d <path_to_collection_directory> -l <lang_code> [options]
```

**Options:**

*   `-l <code`, `--language <code`: **(Required)** Language code for the manga (e.g., `zho`, `jpn`, `eng`).
*   `--prefer-cn-name`: Use the Chinese translated name (name_cn) from Bangumi as the title, if available.
*   `--library-path <path>`: Specify the path to your Calibre library. This can be a local path or a remote server address (e.g., `http://localhost:8080/#mylibrary`).
*   `--dry-run`: Prints the `calibredb` commands that would be executed without actually running them. Useful for testing.
*   `--query-only`: Fetches and prints metadata from Bangumi without attempting to add anything to Calibre. Useful for verifying metadata lookup.

---

### Example Usage

1.  **Adding a single manga series to a local Calibre library (Chinese):**

    ```bash
    ./patchouli add "/home/user/MangaLibrary/Yotsuba - Azuma Kiyohiko [bgm_10086]" -l zho
    ```

2.  **Batch adding a folder of manga series:**

    ```bash
    ./patchouli add -d /home/user/MangaLibrary -l zho
    ```
    *Assuming `/home/user/MangaLibrary` contains subfolders like `Title - Author`.*

3.  **Adding to a remote Calibre Content Server, preferring Chinese titles:**

    ```bash
    ./patchouli add -d /mnt/network/Manga --library-path "http://mycalibreserver.com:8080/#main_manga_library" --prefer-cn-name -l zho
    ```

4.  **Performing a dry run to see commands without execution:**

    ```bash
    ./patchouli add -d /path/to/my/manga -l zho --dry-run
    ```

5.  **Querying metadata for a collection without adding to Calibre:**

    ```bash
    ./patchouli add -d /tmp/NewMangaDownloads -l zho --query-only
    ```

## Development & Contribution

The project's source code is available on [GitHub](https://github.com/Avimitin/patchouli). Contributions, bug reports, and feature requests are welcome.