# Multi line strins in yaml

When it comes to multi line strings in yaml we can use block style (Block Scalars) we there two options.

```yaml
multi_line_strings:
    stringone: |
        this is a string where the | will
        preserved all newline characters meaning
        when parsed it will look as it is.
    stringtwo: >
        this is a string where > will
        convert newline characters to spaces meaning
        when parsed it will be all on one line.
```

This depends on how you want the message / string returned...

## Chopping indicators

Excellent question — the **`+`** in YAML block scalars is the **opposite** of the `-`.

Let’s go over all three chomping indicators (`|`, `|-`, and `|+`) and when you’d use each.

---

### 🧩 The “chomping” indicators in YAML

#### 1. `|` — *Keep one final newline (default)*

```yaml
example_default: |
  Hello
  World
```

✅ Parsed as: `"Hello\nWorld\n"`
This is the **default** behavior — YAML keeps **exactly one newline** at the end.

📘 **Use case:**
When you want your block of text to look natural — most human-readable text (like logs, config snippets, or paragraphs) should end with one newline.

---

#### 2. `|-` — *Chop the final newline (remove it)*

```yaml
example_chop: |-
  Hello
  World
```

✅ Parsed as: `"Hello\nWorld"`

📘 **Use case:**
When the newline at the end would cause a problem — for example:

* When embedding a string into JSON or a single-line format where trailing newlines are not desired.
* When the content must match exactly (like checksums or digital signatures).
* When working with templates that would break with an extra newline.

---

#### 3. `|+` — *Keep all trailing newlines (preserve)*

```yaml
example_keep_all: |+
  Hello
  World


```

✅ Parsed as: `"Hello\nWorld\n\n"` (two newlines kept)

📘 **Use case:**
When trailing blank lines **matter** — for example:

* Generating Markdown, text files, or shell scripts that require extra blank lines at the end.
* Maintaining strict formatting for documentation or templated outputs.

---

### 💡 Summary Table

| Scalar | Meaning | Keeps internal newlines?    | Keeps trailing newlines? | Typical Use |                                       |
| :----- | :------ | :-------------------------- | :----------------------- | :---------- | ------------------------------------- |
| `      | `       | Literal block               | ✅ Yes                    | 1 newline   | Normal text                           |
| `      | -`      | Literal block, **chop**     | ✅ Yes                    | ❌ None      | JSON/text without trailing newline    |
| `      | +`      | Literal block, **keep all** | ✅ Yes                    | ✅ All       | Markdown, files where spacing matters |

---

### Numbers

**the indentation indicator** in YAML block scalars.
It’s written **as a number** right after the `|` or `>` (and before any `+`/`-` chomping indicator).

Let’s break that down.

---

### 🧱 1. Syntax overview

A block scalar can look like this:

```
|[indent][chomping]
>[indent][chomping]
```

Where:

* `|` → literal block (preserve newlines)
* `>` → folded block (fold newlines into spaces)
* `[indent]` → an **integer** (1–9) telling YAML **how many spaces to strip** from each line
* `[chomping]` → `+`, `-`, or nothing (as we discussed earlier)

---

### 📘 Example: indentation indicator

```yaml
example: |2
    This line has 4 spaces in the file,
    but YAML will strip 2 from each.
```

The scalar value is parsed as:

```
"  This line has 4 spaces in the file,\n  but YAML will strip 2 from each.\n"
```

✅ So it preserves 2 leading spaces from each line (4 – 2 = 2 left).

---

### 🧩 2. Why use indentation indicators?

Normally, YAML determines indentation automatically from the first line of content, but **you can specify it manually** when:

* Your editor or code formatter makes it hard to line up blocks exactly.
* You’re embedding YAML inside another structure (e.g., Helm charts, templating).
* You need predictable indentation for generated files or scripts.

---

### 🔍 3. Combined examples

You can combine **indentation** and **chomping** indicators:

```yaml
example_keep: |+2
    Line 1
    Line 2


example_chop: >-4
        This is folded text
        with four-space indentation
```

* `|+2` → literal block, strip **2 spaces** from each line, **keep** all trailing newlines
* `>-4` → folded block, strip **4 spaces**, and **remove** final newline

---

### 💡 Quick reference table

| Indicator | Meaning                                                  |                                                   |
| :-------- | :------------------------------------------------------- | ------------------------------------------------- |
| `         | 2`                                                       | Literal block, strip 2 spaces from each line      |
| `         | -4`                                                      | Literal block, strip 4 spaces, chop final newline |
| `>2`      | Folded block, strip 2 spaces, keep one newline           |                                                   |
| `>+2`     | Folded block, strip 2 spaces, keep all trailing newlines |                                                   |

---