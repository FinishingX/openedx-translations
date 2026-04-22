# Open edX Translation Customization Guide

## FinishingX LMS Platform

This guide explains how to customize translations on the FinishingX Open edX LMS platform. Follow these steps to modify any label, sentence, button, or UI text across the platform.

**Live Platform:** [https://finishingx.de/](https://finishingx.de/)
**Translation Repository:** [https://github.com/FinishingX/openedx-translations.git](https://github.com/FinishingX/openedx-translations.git)
**Active Branch:** `finishingx/ulmo.2`
**Primary Language:** German (`de`)

---

## 1. Overview

The [openedx-translations](https://github.com/openedx/openedx-translations) repository is the official source of all translation files used by the Open edX platform. We maintain a custom fork at **[FinishingX/openedx-translations](https://github.com/FinishingX/openedx-translations)** where all FinishingX-specific translation changes are made.

Using this repository, you can customize translations for:

- **LMS Backend** (edx-platform Django `.po` files)
- **LMS JavaScript** (edx-platform `djangojs.po` files)
- **Micro-Frontends (MFEs)** such as Learning, Authentication, Profile, Account, Discussions, etc. (JSON files)

---

## 2. Prerequisites

Before you begin, ensure you have:

- A GitHub account with access to the `FinishingX/openedx-translations` repository
- Git installed on your local machine
- A basic text editor (VS Code recommended)
- SSH access to the server where Tutor is installed (for rebuild)

---

## 3. Getting Started

### Step 1 — Clone the Repository

The repository is already forked to `FinishingX/openedx-translations`. Clone it locally:

```bash
git clone https://github.com/FinishingX/openedx-translations.git
cd openedx-translations
```

### Step 2 — Checkout the Active Branch

Always work on the `finishingx/ulmo.2` branch:

```bash
git checkout finishingx/ulmo.2
git pull origin finishingx/ulmo.2
```

### Step 3 — Create a Working Branch (Recommended)

To keep changes safe and reviewable, create a feature branch before editing:

```bash
git checkout -b update/german-translations
```

---

## 4. Repository Structure

All translation files live inside the `translations/` directory.

### A. LMS / Django Translation Files

Backend and LMS UI translations are located here:

```
translations/edx-platform/conf/locale/<language-code>/LC_MESSAGES/
```

For German, the path is:

```
translations/edx-platform/conf/locale/de/LC_MESSAGES/
```

Inside this folder you will find:

| File | Purpose |
|------|---------|
| `django.po` | Backend / server-side translations (most UI text) |
| `djangojs.po` | JavaScript translations used inside the LMS |

### B. MFE Translation Folders (JSON)

Each Micro-Frontend has its own folder. The ones most relevant to FinishingX are:

| Folder | Description |
|--------|-------------|
| `frontend-app-authn` | Login, Register, Password Reset screens |
| `frontend-app-account` | Account settings and profile preferences |
| `frontend-app-profile` | User profile page |
| `frontend-app-learning` | Course content / learner view |
| `frontend-app-discussions` | Discussion forum inside courses |
| `frontend-app-course-authoring` | Studio / course authoring interface |
| `frontend-app-gradebook` | Gradebook |
| `frontend-app-learner-dashboard` | Learner dashboard |

Inside each MFE folder:

```
src/i18n/messages/<language-code>.json   → Translated strings (edit this)
src/i18n/transifex_input.json            → Original English strings (reference only)
```

For German, edit the file named `de.json`.

> **Note:** Some older `frontend-app-*` folders are legacy and no longer used by the platform. You can ignore them.

---

## 5. Editing Django `.po` Files (LMS Backend)

### File location

```
translations/edx-platform/conf/locale/de/LC_MESSAGES/django.po
```

### How `.po` entries work

Each entry has two lines:

```po
msgid "Staff"
msgstr "Mitarbeiter"
```

- `msgid` → the **original English string**. **Never change this.**
- `msgstr` → the **translated string**. This is the only line you edit.

### Example 1 — Rename a label

To change how "Staff" appears in German from "Mitarbeiter" to "Lehrkraft":

```po
msgid "Staff"
msgstr "Lehrkraft"
```

### Example 2 — Update a validation message

```po
msgid "must have name of the configuration"
msgstr "Der Konfigurationsname ist erforderlich"
```

### Rules

- ✅ Always keep `msgid` exactly the same.
- ✅ Only edit the text inside `msgstr`.
- ✅ Preserve placeholders like `%(name)s`, `{course}`, `%s` — they must appear in the translated string too.
- ❌ Do not remove empty `msgstr ""` entries — leave them empty if you do not want to translate that string.

---

## 6. Editing MFE JSON Files

### File location

Example for the Account MFE in German:

```
translations/frontend-app-account/src/i18n/messages/de.json
```

### How JSON entries work

```json
"account.settings.field.full.name.help.text.default": "Der Name, der in Ihrem öffentlichen Profil angezeigt wird."
```

- The **key** (left side) is a unique identifier. **Never change it.**
- The **value** (right side, in quotes) is the translated text. Edit only this.

### Example — Change the help text

To update the help text for the full name field:

```json
"account.settings.field.full.name.help.text.default": "Dieser Name erscheint auf Ihrem öffentlichen FinishingX-Profil."
```

### Rules

- ✅ Keep the JSON valid — every line except the last inside a block ends with a comma.
- ✅ Escape double quotes inside a value using `\"`.
- ❌ No trailing commas after the last entry.
- ❌ Do not change the keys.

> 💡 **Tip:** Open the file in VS Code with the JSON extension enabled. It will highlight syntax errors immediately.

---

## 7. Page-to-File Mapping (German)

Use this table to find the correct translation file for the page you want to modify. Replace `finishingx.de` with your working environment if different.

| Page / URL | File Location | File Name |
|------------|---------------|-----------|
| Homepage `https://finishingx.de/` | `translations/edx-platform/conf/locale/de/LC_MESSAGES/` | `django.po` |
| Dashboard `/dashboard` | `translations/edx-platform/conf/locale/de/LC_MESSAGES/` | `django.po` |
| Course list `/courses` | `translations/edx-platform/conf/locale/de/LC_MESSAGES/` | `django.po` |
| Course details `/courses/course-v1:.../about` | `translations/edx-platform/conf/locale/de/LC_MESSAGES/` | `django.po` |
| Admin panel `/admin/` | `translations/edx-platform/conf/locale/de/LC_MESSAGES/` | `django.po` |
| Logout `/logout` | `translations/edx-platform/conf/locale/de/LC_MESSAGES/` | `django.po` |
| Login `/authn/login` | `translations/frontend-app-authn/src/i18n/messages/` | `de.json` |
| Register `/authn/register` | `translations/frontend-app-authn/src/i18n/messages/` | `de.json` |
| Password reset `/authn/reset` | `translations/frontend-app-authn/src/i18n/messages/` | `de.json` |
| User profile `/profile/<username>` | `translations/frontend-app-profile/src/i18n/messages/` | `de.json` |
| Account settings `/account/account-settings` | `translations/frontend-app-account/src/i18n/messages/` | `de.json` |
| Course authoring `/authoring/home` | `translations/frontend-app-course-authoring/src/i18n/messages/` | `de.json` |
| Course content `/learning/course/.../home` | `translations/frontend-app-learning/src/i18n/messages/` | `de.json` |
| Discussions `/discussions/course-v1:.../` | `translations/frontend-app-discussions/src/i18n/messages/` | `de.json` |

> If you are not sure which file a string belongs to, search the whole `translations/` folder for the original English text using your editor's global search (`Ctrl + Shift + F` in VS Code).

---

## 8. Commit and Push Your Changes

Once you have edited the required files:

```bash
git add .
git commit -m "Update German translations for <describe what you changed>"
git push origin update/german-translations
```

Open a Pull Request on GitHub to merge your branch into `finishingx/ulmo.2`.

---

## 9. Apply Changes on the Server

After the PR is merged into `finishingx/ulmo.2`, the server needs to pull the new translations and rebuild.

### Step 1 — Point Tutor to the FinishingX repo

Run these commands on the server (only needed the first time, or if values change):

```bash
tutor config save --set ATLAS_REVISION=finishingx/ulmo.2
tutor config save --set ATLAS_REPOSITORY=FinishingX/openedx-translations
```

### Step 2 — Rebuild and restart

For production:

```bash
tutor images build openedx
tutor local restart
```

For development:

```bash
tutor dev restart
```

### Step 3 — Verify

- Clear your browser cache or open an incognito window.
- Navigate to the page you changed on [https://finishingx.de/](https://finishingx.de/).
- Confirm the new text appears.

---

## 10. Best Practices

1. **Always work on a feature branch** — never commit directly to `finishingx/ulmo.2`.
2. **Keep `msgid` and JSON keys unchanged** — these are identifiers, not text.
3. **Preserve placeholders** like `%(username)s`, `{count}`, `%s`. They are replaced at runtime with real values.
4. **Validate JSON files** before committing — a single misplaced comma will break the entire MFE.
5. **Test locally when possible** before pushing to production.
6. **Use meaningful commit messages** so changes can be traced later.
7. **Do not edit `transifex_input.json`** — it contains the English source strings.
8. **Do not touch deprecated MFE folders** (check with the tech team if unsure).

---

## 11. Troubleshooting

| Issue | Likely Cause | Fix |
|-------|-------------|-----|
| Translation didn't appear after rebuild | Browser cache | Hard refresh (`Ctrl + Shift + R`) or open incognito |
| MFE page shows blank or errors | Invalid JSON (trailing comma, missing quote) | Validate the `de.json` file with a linter |
| `.po` change had no effect | Edited the wrong file (e.g., `en` instead of `de`) | Confirm you edited the `de/LC_MESSAGES/` file |
| Build fails on Tutor | `ATLAS_REVISION` or `ATLAS_REPOSITORY` mis-set | Re-run the `tutor config save` commands in Section 9 |
| Placeholder shows as `%(name)s` in UI | Placeholder was removed from `msgstr` | Re-add the exact placeholder in the translated text |

---

## 12. Quick Reference

- **Fork used:** `FinishingX/openedx-translations`
- **Branch:** `finishingx/ulmo.2`
- **Language code:** `de` (German)
- **Main Django file:** `translations/edx-platform/conf/locale/de/LC_MESSAGES/django.po`
- **MFE JSON pattern:** `translations/<mfe-name>/src/i18n/messages/de.json`

For questions or merge approvals, contact the FinishingX platform team.