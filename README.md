# Patchouli CLI

A command-line tool designed to help manga collectors integrate their local manga files with Calibre, leveraging metadata from [Bangumi (bgm.tv)](https://bgm.tv/).

## Features

*   **Manga Directory Scanning:** Scans user-specified directories for manga series.
*   **Bangumi Metadata Integration:** Extracts comprehensive metadata (titles, authors, tags, cover art) from bgm.tv.
*   **Volume-Specific Metadata:** Utilizes Bangumi's relation system to fetch and apply metadata unique to individual volumes within a series, such as specific volume titles and cover art, while maintaining proper series grouping in Calibre.
*   **Calibre Library Management:** Adds manga files to your Calibre library, supporting both local and remote Calibre Content Servers.
*   **Strict Naming Conventions:** Enforces a clear directory naming standard for accurate metadata matching.
*   **Robust Error Handling:** Exits early on network request failures or Calibre database operation errors.

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

For `patchouli` to accurately identify your manga series and fetch correct metadata, organize your manga files within directories that follow one of these strict naming conventions:

*   **`Manga Name - Author Name [bgm_ID]`**: Provides the most precise match, using the Bangumi ID directly.
    *   Example: `よつばと! - あずまきよひこ [bgm_10086]`
*   **`Manga Name - Author Name`**: Searches Bangumi using both the manga name and author.
    *   Example: `進撃の巨人 - 諫山創`

Any directories not conforming to these patterns will be skipped.

## Usage

### Add Manga to Calibre

The primary command `add` is used to scan a directory containing your manga series folders and add them to Calibre.

```bash
./patchouli add <path_to_manga_collection> [options]
```

**Arguments:**

*   `<path_to_manga_collection>`: The path to the parent directory containing your manga series folders.

**Options:**

*   `--prefer-cn-name`: Use the Chinese translated name (name_cn) from Bangumi as the title, if available.
*   `--library-path <path>`: Specify the path to your Calibre library. This can be a local path or a remote server address (e.g., `http://localhost:8080/#mylibrary`).
*   `--dry-run`: Prints the `calibredb` commands that would be executed without actually running them. Useful for testing.
*   `--query-only`: Fetches and prints metadata from Bangumi without attempting to add anything to Calibre. Useful for verifying metadata lookup.

---

### Example Usage

1.  **Adding manga to a local Calibre library:**

    ```bash
    ./patchouli add /home/user/MangaLibrary
    ```

2.  **Adding manga to a remote Calibre Content Server, preferring Chinese titles:**

    ```bash
    ./patchouli add /mnt/network/Manga --library-path "http://mycalibreserver.com:8080/#main_manga_library" --prefer-cn-name
    ```

3.  **Performing a dry run to see commands without execution:**

    ```bash
    ./patchouli add /path/to/my/manga --dry-run
    ```

4.  **Querying metadata for a collection without adding to Calibre:**

    ```bash
    ./patchouli add /tmp/NewMangaDownloads --query-only
    ```

## Development & Contribution

The project's source code is available on [GitHub](https://github.com/Avimitin/patchouli). Contributions, bug reports, and feature requests are welcome.
