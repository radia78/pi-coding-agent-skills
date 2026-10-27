---
name: plan-code
description: Generate a concise and basic plan for writing any script or code. Use to create any basic plan before writing any script or code.
---

# Plan Code

The plan must include a goal, functions/modules in the code, and the list of frameworks/language that will be used (usually specified by the user).

## Steps

1. Take the goal, language, and frameworks from what the user already said. Don't re-ask anything that's stated.
2. If the language or framework isn't specified, pick the simplest fit (standard library first) and state it as an assumption so the user can correct it. Ask a question only if a wrong guess would waste real effort.
3. Write the plan using the format below.
4. Save the plan as `plans/{name-of-plan}.md`. The user will specify the appropriate plan name, otherwise decide on an appropriate plan name.

## Plan format

Use exactly these three sections, in this order:

**Goal**
One or two sentences: what the code does and what "done" looks like.

**Functions / Modules**
A short list. Each item gets a name and a one-line purpose, in the order they run or are called.

**Language & Frameworks**
The language (with version if it matters) and any libraries or frameworks, each with a few words on why it's needed. Note whether the user specified it or you assumed it.

## Guidelines

- Keep the whole plan short, ideally under 15 lines. It should never be longer than the code it describes.
- Be concrete: `load_csv(path)` and `clean_emails(df)` beat "data handling".
- For tiny scripts, a single function or module is fine; don't invent structure the code doesn't need.
- Add nothing beyond the three sections unless the user asks, with one exception: if there's a serious risk (such as deleting or overwriting files), add a one-line warning at the end.

## Example

**User:** Write a Python script that renames photos by the date they were taken.

**Goal**
Rename every photo in a folder to `YYYY-MM-DD_HHMMSS.ext` using its capture date, without overwriting any file.

**Functions / Modules**
- `get_photo_date(path)`: read the EXIF date, falling back to the file's modified time
- `build_new_name(date, ext, existing)`: format the name and resolve duplicates with a `_1`, `_2` suffix
- `rename_photos(folder, dry_run)`: loop through files and apply the renames
- `main()`: parse the folder path and the `--apply` flag

**Language & Frameworks**
- Python 3 (assumed)
- Pillow: read EXIF data
- `argparse`, `pathlib` (standard library): CLI and file handling

Note: runs as a dry run by default; `--apply` performs the renames.