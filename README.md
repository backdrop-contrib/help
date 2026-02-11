# Help

Restores the Help pages that were removed from Backdrop core.

## Why it exists

Backdrop core dropped the old Help pages and the `hook_help()` display as part of the cleanup outlined in the [features removed from core](https://docs.backdropcms.org/documentation/features-removed-from-core) document. This contrib module brings that experience back so module authors can surface guidance in the UI again.

## What it does

- Recreates the `/admin/help` listing page.
- Shows per-module help at `/admin/help/<module>` using either `hook_help()` output or the module’s README.md file.
- Respects enabled modules only; hidden modules without help content are skipped.

## Usage

1. Enable the Help module.
2. Visit `admin/help` to see available topics sourced from enabled modules.
3. Click any module name to view its help text or README.md file.

## Notes

- Enable the contrib [Markdown](https://backdropcms.org/project/markdown) module to have README files render with full Markdown formatting; without it they fall back to plain text with basic line breaks.
- `hook_help()` output is shown as-is; if it is plain text and Markdown is enabled, it will be rendered as Markdown.

## Using `hook_help()`

Backdrop core no longer renders `hook_help()`, but this module restores that behavior. To surface contextual help for your module:

1. Implement `hook_help()` in your module file (signature: `function example_help($path)`).
2. Return help for the path `admin/help#<module_name>`.
3. Keep the output concise; Markdown will be applied if the Markdown module is enabled and your help text is plain.

Example:

```php
/**
 * Implements hook_help().
 */
function example_help($path) {
 if ($path == 'admin/help#example') {
  return '<p>' . t('The Example module demonstrates how to provide help text.') . '</p>'
   . '<p>' . t('Find settings at @link.', array('@link' => l(t('Admin > Config > Example'), 'admin/config/development/example'))) . '</p>';
 }
}
```

Tips:

- Use the `$path` check shown above; other paths may call `hook_help()` for contextual tooltips.
- If your help text is plain (no HTML tags), it will be rendered through Markdown (when enabled) and sanitized.
- You can also provide a README.md; it will be used when `hook_help()` returns nothing.

## Maintainers

This module is created and maintained by [Alan Mels](https://github.com/alanmels) of [AltaGrade.com](https://www.altagrade.com). Contributions and issue reports are welcome.
