# zabrze

[![](https://github.com/Ryooooooga/zabrze/actions/workflows/build.yml/badge.svg)](https://github.com/Ryooooooga/zabrze/actions/workflows/build.yml)
[![](https://badgen.net/crates/v/zabrze)](https://crates.io/crates/zabrze)

ZSH abbreviation expansion plugin

## Usage

### Simple abbreviation

```yaml
# ~/.config/zabrze/config.yaml
abbrevs:
  - name: git
    abbr: g
    snippet: git

  - name: awk '{print $1}'
    abbr: '.1'
    snippet: awk '{print $1}'
```

```zsh
$ eval "$(zabrze init --bind-keys)"
```

then

```zsh
$ g<SP>cm<SP>
#  ↓ expanded
$ git commit -m 

$ cat a.txt | .1<CR>
#  ↓ expanded and executed
$ cat a.txt | awk '{print $1}'
```

### Global abbreviation

```yaml
abbrevs:
  - name: '>/dev/null'
    abbr: 'null'
    snippets: '>/dev/null 2>&1'
    global: true
```

```zsh
$ echo a null<SP>
#  ↓ expanded
$ echo a >/dev/null 2>&1
```

### Global abbreviation with context

```yaml
abbrevs:
  - name: git commit
    abbr: c
    snippet: commit
    global: true
    context: '^git\s'

  - name: branch name
    abbr: B
    snippet: $(git symbolic-ref --short HEAD)
    evaluate: true
    global: true
    context: '^git\s'
```

```zsh
$ git c<SP>
#  ↓ expanded
$ git commit

$ git push -d origin B<CR>
#  ↓ expanded and executed
$ git push -d origin main
```

### Conditional abbreviation

```yaml
abbrevs:
  - name: chrome
    abbr: chrome
    snippet: open -a 'Google Chrome'
    if: '[[ "$OSTYPE" =~ darwin ]]' # only available in macOS
```

### Suffix alias

```yaml
abbrevs:
  - name: python3 *.py
    abbr-pattern: \.py$
    snippet: python3
    action: prepend
```

```zsh
$ ./a.py<CR>
#  ↓ expanded and executed
$ python3 ./a.py
```

## Installation

### From prebuilt binary

You can download a binary release [here](https://github.com/Ryooooooga/zabrze/releases).

### zinit

```zsh
zinit blockf light-mode as"program" from"gh-r" for \
    atload'eval "$(zabrze init --bind-keys)"' \
    Ryooooooga/zabrze
```

### Cargo

```zsh
$ cargo install zabrze
```

### Homebrew

```zsh
$ brew install ryooooooga/tap/zabrze
```

## Alternatives

- [zsh-abbrev-alias](https://github.com/momo-lab/zsh-abbrev-alias)
- [zeno.zsh](https://github.com/yuki-yano/zeno.zsh)
