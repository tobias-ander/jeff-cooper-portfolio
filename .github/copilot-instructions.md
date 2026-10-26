---
description: 'Specific rules for the code review agent on Pull Requests.'
excludeAgent: "coding" 
---

# Code Review Agent Instructions
- Prefer findings in files modified by the Pull Request.
- Do not flag existing issues outside the changed lines unless they introduce new risk.

## Review Principles
- Focus on correctness, accessibility, security, performance, and maintainability.
- Prefer high-signal findings over stylistic preferences.
- Do not report issues that cannot be reasonably verified from the PR.
- Avoid speculative recommendations.
- Only make recommendations with clear engineering value.

## Priority Levels
### Critical
- Accessibility violations preventing keyboard or screen-reader use
- Broken forms
- Security issues
- Missing alt text
### Important
- Semantic HTML issues
- Missing Open Graph tags
- Large image sizes
### Suggestion
- Formatting consistency
- Naming conventions
- CSS selector preferences

## Avoid False Positives
Do not comment on:
- Generated files
- Vendor code
- Third-party libraries
- Build artifacts
- Minified files
- Code outside the changed files unless directly affected

## General
- Verify repository structure follows the project's conventions and does not place application code inside `.github` unless intentionally required.
- Check for any spelling errors

## .gitignore
- Check that there is a `.gitignore` file present
- Check that the `.gitignore` file has the most commonly ignored files defined

## HTML
### General
- Check that the `lang` attribute in the `<html>` tag is consistent with the page language.
- Check that semantic tags are used as widely as possible.
- Check if there are any unnecessary `tabindex` attributes set.

### Image tags
- Ensure images have a descriptive `alt`-text set and that the text doesn't contain "Picture of" or "Bild på".
- Ensure images have `width` and `height` attributes set.
- If the image tag is missing `loading="lazy"`, recommend it to be added.
- Check that there is only one `<h1>` tag.
- Check that no heading levels are skipped.
- If large blocks of text are written in uppercase solely for visual styling, recommend using CSS text-transform instead.
- If there is a menu button, check that it is a `<button>` element and that it contains text, either by `aria-label` or actual text inside the element.
- Flag images larger than 1 MB and recommend optimization.
- Flag images larger than 2 MB as a high-priority performance concern.
- If a PNG has no transparency, recommend evaluating WebP or AVIF before JPG.

### Forms
- Ensure all form fields have connected labels, either through the `id` property or by nesting the form field inside the `<label>` tag.
- Ensure all form fields have the correct type set, based on their label. For example if the label is "email", the corresponding `<input>` `type` should be `email`.

### Accessibility
- Check the HTML for accessibility
- Check that the main navigation uses the `<nav>` tag.
- Inside forms, check that the submit button is `<button type="submit">` and not a `<input type="submit">` element.
- If Font Awesome is present, i.e. there's an `<i class="fa">` or `<i class="fa-solid">` element present, check that it has the attribute `aria-hidden="true"` set, otherwise recommend the author to read up on https://fontawesome.com/v4/accessibility/.
- Ensure interactive elements are keyboard accessible.
- Ensure buttons have an accessible name.
- Check for sufficient color contrast when colors are defined in the PR.
- Verify form validation messages can be understood by screen readers.
- Ensure `aria-*` attributes are used correctly and not unnecessarily.


### Formatting
- Check the HTML document for correct indentation. If it's not correctly formatted, recommend the author to watch the following video: https://www.youtube.com/watch?v=SPUDVy11FCk

### Best practices and coding conventions
- Check that there are no duplicate or unneccessary `id` attributes on HTML elements that do not have a corresponding tag for navigation.
- Flag any `<br>` tags, and check if they are required or if CSS could be used instead to create the spacing.
- Flag use of `<i>` and `<b>` tags.
- Flag use of any deprecated HTML tags.
- If there is a phone number present, check that the anchor tag has a `tel` prefix.
- Ensure all HTML elements are in lowercase.
- Ensure all external links have the `target="_blank"` attribute set.
- Ensure all external links declared with an `<a>` tags have security applied by having the `rel="noopener noreferrer"` attribute and attribute value present.

### SEO
- Check that the page has a unique title tag.
- Check that a meta description is present.
- Check that Open Graph metadata is present when applicable.
- Check that images used for sharing have suitable dimensions.
- Check for canonical URLs when multiple URLs may exist.

### Fonts
- If Google Fonts have been added, check that there are no duplicate `<link>` tags in `<head>`.

## Image files
- Check that images follow web naming standards; no spaces, all lowercase, special characters and no international (umlaut) characters
- Notify if any image file exceeds 1 MB in size
- If the image is in PNG format, analyze if it has any transparency, otherwise recommend it to be converted to a more compressed format like JPG.
- If the image is used as a profile picture, flag if the filename is not `firstname_lastname` and mention SEO benefits of naming image files correctly.

## CSS
- Check that the document is correctly formatted/indented. If it is not, recommend the author to watch the following video: https://www.youtube.com/watch?v=SPUDVy11FCk.
- Flag any uses of `id` selectors
- Recommend using a single language convention for selectors throughout the project.
- Check that all selectors follow the same convention, like "camel case", "snake case", or "kebab case".
- If the selector is in Pascal Case, recommend to use camel case, snake case or kebab case instead.
- Check that CSS values that are zero do not have a suffix, i.e. `0px` should be `0`.
- Ensure `url()` values have quotation marks around the URL.
- Check that the CSS files contain no `@import` declarations, unless it's `@import "tailwindcss"`.
- If there is a `@import "tailwindcss"`, check that it is at line 1 of the file.
- Check that `font-size` is set in `rem` or `em`, not `px` units.
- If there is a `height` attribute set with a value that isn't `auto`, recommend using `max-height` instead to avoid cutting off or overflowing content.
- Make sure all fonts have a backup font defined.
- If the repository contains the name `fed`, recommend using `srcset` for images for maximal responsivity and optimization.
- Ensure consistency in font size units, i.e. all font units should be either `rem` or `em` throughout all CSS/SCSS/LESS documents.
- Check for consistency in formatting in the document, i.e. that there is a space between the selector name and the curly brace. The document should not contain one selector looking like this `html, body {` (with space) and another one looking like this: `@media (max-width: 1200px){` (without space).

### Sass/SCSS
- If SCSS/Sass has been used in the project, check that no `*.css` file has been committed. If it has been committed, recommend to add it to `.gitignore` instead.

## README file
- Check that the `README.md` file contains some sort of description of what the repository is about, and no generic classroom assignment instructions.

## Security
- Check for hard-coded credentials, secrets, API keys, or tokens.
- Check for exposed email addresses when not required.
- Check for use of unsafe innerHTML assignments.
- Check for user input rendered without sanitization.
- Check for missing rel="noopener noreferrer" on external links opened in a new tab.

## Code Quality
 
- Prefer maintainability over personal style preferences.
- Flag duplicated code introduced by the PR.
- Flag unnecessary complexity.
- Flag dead code that is introduced.
- Verify naming is consistent with surrounding code.

## Review Format

For each finding provide:

- Severity: Critical | Important | Suggestion
- File
- Line number
- Description
- Recommended change

Only report findings that are actionable and specific.
Do not report speculative issues.
