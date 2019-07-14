# concat-md

CLI and API to concatenate markdown files and modify as necessary.

# Usage

```
$ npx concat-md --decrease-title-levels --file-name-as-title --dir-name-as-title typedoc-api-docs > README.md
```

# Features

- Scans all markdown files in a directory,
- Optionally ignores some files,
- Concatenates all of them,
- Optionally adds titles from `FrontMatter`, file names and directory names,
- Decreases level of existing titles to comply with added titles,

# CLI Options

```bash
Usage
  $ md-merge [options] <dir>

Options
  --ignore <globs csv>              - Glob patterns to exclude in 'dir'.
  --decrease-title-levels           - Whether to decrease levels of all titles in markdown file to set them below file and directory title levels.
  --start-title-level-at <level no> - Level to start file and directory levels. Default: 1
  --join-string <string>            - String to be used to join concatenated files. Default: new line
  --title-key <key name>            - Key name to get title in 'FrontMatter' meta data in markdown headers.
  --file-name-as-title              - Whether to use file names as titles.
  --dir-name-as-title               - Whether to use directory names as titles.
  --debug                           - Print stack trace in errors.

Examples
  $ md-merge docs > README.md
  $ md-merge --decrease-title-levels --file-name-as-title --dir-name-as-title --title-key typedoc-api > README.md
```

# Example

```
└─ Development
   └─ Projects
      └─ project-a
         ├─ docs
         ├─ classes
         │  ├─ widget.md
         │  ├─ utility.md
         └─ interfaces
            └─ screen.md
```

**classes/widget.md**

```md
# Description

Widget details lorem ipsum...
```

**classes/utility.md**

```md
# Description

Utility details lorem ipsum...
```

**interfaces/screen.md**

```md
# Description

Screen details lorem ipsum...
```

```bash
$ md-merge --decrease-title-levels --file-name-as-title --dir-name-as-title docs > README.md
```

Above command;

- Concatenates markdown files,
- Adds directory names and file names as title,
- Decreases level of `Description` titles,
- Outputs to `README.md` as below:

**README.md**

```md
# Classes

## Widget

### Description

Widget details lorem ipsum...

## Utility

### Description

Utility details lorem ipsum...

# Interfaces

## Screen

### Description

Screen details lorem ipsum...
```