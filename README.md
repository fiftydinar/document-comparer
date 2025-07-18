# document-comparer

Document Comparer is a program written in POSIX, which intends to keep the user up-to-date with document changes over time from the internet.

It downloads the documents from the internet, compares them to the documents in the local storage & notifies + shows the user if there are any changes between them.

I haven't found any application of this type, so I wrote it for myself.

## Document format support

- **PDF**
- **DOCX**
- **ODT**
- **EPUB**

## Usage

```
document-comparer /path/to/configuration.yml
document-comparer /path/to/configuration1.yml /path/to/configuration2.yml
```

- Documents are downloaded in the same directory where configuration is.
- Downloaded files in `.txt` format MUST remain aside from the original documents for future comparisons to work.

### Configuration example

Program accepts required options `title:` & `document-links:` in `.yml` & `.yaml` configuration:
  - `title`: serves as a general description of the document types which are being compared, so it's easier to differentiate in case of more configurations
  - `document-links`: serves as an URL list of documents which are going to be compared

**/path/to/configuration.yml**:
```yaml
title: Work stuff
document-links:
  - link1.com/file.pdf
  - link2.com/file2.odt
```

### Automatic notification on selected time period

Due to inclusion of `notify-send` / `terminal-notifier` usage in the program, depending on the init & timer system, it's easy to integrate the program so Document Comparer can notify you about the changes automatically.

## Program compatibility

This program should work on any Linux & BSD-based OS-es, including MacOS, if the following dependencies below are installed.

### Program dependencies

#### Required dependencies
- **id**: checks if the program is ran as root.
- **diff**: compares the difference between old & new version of the document.
- **pdftotext**: Converts PDF documents to TXT.
- **pandoc**: Converts DOCX, ODT & EPUB documents to TXT.
- **curl**: checks the internet connection & downloads documents.
- **sed**: gets values from YML/YAML file, converts Cyrillic/whitespace coded URL filename to normal one & is used for checking if flatpak Meld is installed on Linux.  
  It is tested that `sed`'s syntax in the program works with GNU, Busybox & BSD's `sed`.
- **xargs**: passes the `printf` argument for the URL operation above, as POSIX shell doesn't support the argument syntax.
- **mv**: overwrites the old document TXT with the new one.

#### Recommended dependencies (for better UX)
- **notify-send (Linux & BSD) / terminal-notifier (MacOS)**: for getting notifications of document changes & errors.
  - **Fallback**: `printf` in terminal.
- **meld**: for a nice GUI application showcasing the difference between old & new version of the document.
  - **Linux**: flatpak is preferred if available, if not, then regular program `meld` in `PATH`.
  - **BSD**: regular program `meld` in `PATH`.
  - **MacOS**: Meld DMG package to be installed either in system's or user's `Applications` directory.
  - **Fallback**: messy `diff` output if none of the conditions are satisfied.
- **open (MacOS)**: Used for opening GUI applications (in this case Meld).

Most of these dependencies are installed by default in all those OS-es except `meld`, `pdftotext` & `pandoc`. Additionally, for MacOS, `terminal-notifier` is likely not installed by default.

## Caveats / What doesn't work

- URLs which don't have the filename in the URL are not accepted
- YML/YAML array syntax must be in `- arrayvalue`, not in brackets `[ arrayvalue ]`
