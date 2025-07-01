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

Due to inclusion of `notify-send` usage in the program, depending on the init & timer system, it's easy to integrate the program so Document Comparer can notify you about the changes automatically.

## Program compatibility

This program should work on any Linux, BSD based OS-es, including MacOS, if the following dependencies below are installed.

### Program dependencies

#### Required dependencies
- **diff**: compares the difference between old & new version of the document
- **pdftotext**: Converts PDF documents to TXT
- **pandoc**: Converts DOCX, ODT & EPUB documents to TXT
- **curl**: checks the internet connection & downloads documents
- **sed**: gets values from YML/YAML file & converts Cyrillic coded URL name to normal one.
  It is tested that `sed` syntax in the script works with GNU, Busybox & BSD `sed`
- **xargs**: passes the `printf` argument for the URL operation above, as POSIX shell doesn't support the argument syntax.
- **mv**: overwrites the old document TXT with the new one.

#### Recommended dependencies (for better UX)
- **notify-send (Linux & BSD) / terminal-notifier (MacOS)**: for getting notifications of document changes & errors (`printf` in terminal is there if it's not available)
- **meld**: for a nice GUI application (either in flatpak or a regular one in `PATH`) showcasing the difference between old & new version of the document (fallbacks to messy `diff` output if not available)

## Caveats / What doesn't work

- URLs which don't have the filename in the URL are not accepted
- YML/YAML array syntax must be in `- arrayvalue`, not in brackets `[ arrayvalue ]`
