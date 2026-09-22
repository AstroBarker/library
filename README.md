# library

Personal reference library managed with [Papis](https://papis.readthedocs.io/).

## Layout

```text
library/
├── contents/
│   ├── aarnio.a.2011b
│   │   └── info.yaml
│   │   └── paper.pdf   # not tracked by Git
│   │   ├── notes.md
│   ├── abadie.j.2010b
│   │   └── info.yaml
│   └── ...
└── README.md
```

Each reference is a Papis document directory. `info.yaml` contains the
bibliographic entry.

Citation keys generally follow

```text
surname.given.year
```

dudplicates are appended with letters, e.g., `doe.jane.2026a` is the second
entry added for Jane Doe in 2026. Ideally this is chronological, but 
it is not necessary.

Papis' database/cache is not part of this repository. 
It is derived from the `info.yaml` files and can be rebuilt locally.

## Papis Configuration

The library is configured separately in `${HOME}/.config/papis/config`.

A representative configuration is:

```ini
[settings]
default-library = lex # or whatever

editor = nvim
picktool = fzf
opentool = zathura

ref-word-separator = _

add-subfolder = contents
ref-format = {doc[author_list][0][family]!l}\.{doc[author_list][0][given]!l:0.1S}\.{doc[year]}
add-folder-name = {doc[ref]}
add-file-name = {doc[ref]}

[lex]
dir = ~/library
```

The Papis configuration itself belongs with my normal dotfiles/Home Manager configuration rather than in this repository.

## Adding References

### arXiv

```bash
papis add --from arxiv https://arxiv.org/abs/2609.xxxxx
```

### DOI + local PDF

```bash
papis add ~/Downloads/paper.pdf \
    --from doi https://doi.org/10.xxxx/xxxxx
```

### Existing PDF

```bash
papis add ~/Downloads/paper.pdf
```

## Finding and Reading Papers

Search and open:

```bash
papis open 'radiation shock'
```

Edit metadata:

```bash
papis edit 'radiation shock'
```

Edit notes:

```bash
papis edit --notes 'radiation shock'
```

## Manuscript Workflow

Each manuscript maintains its own committed `references.bib`.

For example:

```text
paper/
├── main.tex
├── references.bib
├── .papis.config
└── ...
```

A project-local `.papis.config` can contain:

```ini
[bibtex]
default-read-bibfile = references.bib
default-save-bibfile = references.bib
auto-read = True
```

Then a reference from the personal library can be added to the manuscript with:

```bash
papis bibtex add -q 'radiation shock' save
```

References not in the Papis library can be imported:

```bash
papis bibtex read references.bib import
```

## Setting Up on a New Machine

Clone the repository:

```bash
git clone <repository> ~/library
```

Configure Papis so that `lex` points to `~/library`.

Test the library:

```bash
papis -l lex list
```
