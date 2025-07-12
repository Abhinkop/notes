## 🔹 Basics
| Pattern  | Meaning                   | Example                                |
| -------- | ------------------------- | -------------------------------------- |
| `.`      | Any single character      | `/c.t` matches `cat`, `cut`            |
| `*`      | 0 or more of the previous | `/lo*se` matches `lse`, `loose`        |
| `\+`     | 1 or more of the previous | `/go\+gle` matches `gogle`, `google`   |
| `\=`     | 0 or 1 of the previous    | `/colou\=r` matches `color`, `colour`  |
| `\{n}`   | Exactly n times           | `/a\{3}` matches `aaa`                 |
| `\{n,}`  | At least n times          | `/a\{2,}` matches `aa`, `aaa`...       |
| `\{n,m}` | Between n and m times     | `/a\{2,4}` matches `aa`, `aaa`, `aaaa` |

## 🔹 Character Classes
| Pattern  | Meaning                                 |
| -------- | --------------------------------------- |
| `[abc]`  | Matches `a`, `b`, or `c`                |
| `[^abc]` | Matches any char *except* `a`, `b`, `c` |
| `[a-z]`  | Range (matches a through z)             |
| `\d`     | Digit (same as `[0-9]`)                 |
| `\D`     | Non-digit                               |
| `\w`     | Word char (alphanumeric + `_`)          |
| `\W`     | Non-word char                           |
| `\s`     | Whitespace                              |
| `\S`     | Non-whitespace                          |

## 🔹 Anchors
| Pattern | Meaning                      |
| ------- | ---------------------------- |
| `^`     | Start of line                |
| `$`     | End of line                  |
| `\<`    | Start of word                |
| `\>`    | End of word                  |
| `\b`    | Word boundary (used in `:s`) |

## 🔹 Grouping & Backreferences
| Pattern    | Meaning                   |
| ---------- | ------------------------- |
| `\(` `\)`  | Group expressions         |
| `\1`       | First group match in `:s` |
| `\2`, `\3` | Second, third group, etc. |

Example:
```bash
:s/\(foo\)bar/\1qux/
```
Changes `foobar` → `fooqux`

## 🔹 Substitution Examples
| Command                            | Description                                 |
| ---------------------------------- | ------------------------------------------- |
| `:%s/foo/bar/g`                    | Replace all `foo` with `bar`                |
| `:%s/\<cat\>/dog/g`                | Replace whole word `cat` with `dog`         |
| `:%s/\s\+$//`                      | Remove trailing whitespace                  |
| `:%s/\(.*\)/"\1",/`                | Add quotes and comma to each line           |
| `:%s/^\(\d\+\)\s\+\(.*\)/\2 (\1)/` | Reformat lines like `123 abc` → `abc (123)` |

## 🔹 Tips
- Use :set hlsearch to highlight matches.

- Use :noh to clear highlights.

- Use \v at the start to enable "very magic" mode (regex like in Perl).

Example:
```bash
/\v(foo|bar)\d+
```
Matches `foo` or `bar` followed by digits.

