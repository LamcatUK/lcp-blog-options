# LC Blog Options WordPress Plugin

A WordPress plugin that provides granular control over blog functionality, allowing administrators to disable blog features, comments, and gravatars through a simple admin interface.

The plugin also forces Advanced Custom Fields blocks to stay in edit mode in the block editor, including immediately after a new block is inserted, and adds a branded Lamcat dashboard widget.

## Features

- **Disable Blog**: Completely removes blog functionality including:
  - Hides Posts menu from admin
  - Prevents access to post-related admin pages
  - Removes blog-related dashboard widgets
  - Disables post type support
  - Automatically disables comments and gravatars

- **Disable Comments**: Removes comment functionality including:
  - Closes comments on all posts
  - Hides existing comments
  - Removes Comments menu from admin
  - Removes comment-related dashboard widgets
  - Hides discussion settings page

- **Disable Gravatars**: Disables avatar/gravatar functionality:
  - Turns off gravatar display
  - Removes avatar options from user profiles

- **Disable Tags**: Detaches post tags from the default `post` post type and hides the tags admin UI

- **Disable Emojis**: Removes WordPress emoji scripts/styles from the frontend and admin, and drops TinyMCE emoji support

- **Suppress Object Cache Warning**: Hides the Site Health "persistent object cache" warning, useful for small sites where object caching is unnecessary

- **Suppress Core Update Nag**: Hides the "WordPress x.x is available" banner and blocks WordPress from auto-updating to a new **major** version on its own — for sites deliberately pinned to an older release. Dashboard > Updates still works, and minor/security updates are unaffected. The major-update block can be lifted independently of the checkbox with `add_filter( 'lcp_block_major_core_auto_updates', '__return_false' )`.

- **Lamcat Dashboard Widget**: Adds a branded admin dashboard widget with quick contact details

- **Force ACF Block Edit Mode**: Keeps all ACF blocks in edit mode in the block editor, including newly inserted blocks. Can be disabled site-wide with `add_filter( 'lcp_acf_blocks_force_edit_mode', '__return_false' )`.

- **WP 7.1 ACF/Gutenberg hardening**: WordPress 7.1 moved the block editor canvas into an iframe, which breaks several things for sites relying on ACF blocks. This plugin works around that automatically:
  - Re-enqueues ACF/editor/button styles inside the iframe canvas so ACF fields and buttons aren't left unstyled
  - Tags the iframe body with `wp-core-ui` so core admin button styles apply inside it
  - Hides ACF's duplicate inspector-sidebar panel when a block is forced into edit mode
  - Fixes a WP 7.1 QTags/`buildQuicktags` crash that could break ACF's field init and show "This block has encountered an error and cannot be previewed"
  - Prevents TinyMCE from stealing focus/scrolling the page when a WYSIWYG field is added inside an ACF repeater or flexible content layout
  - Checking **Suppress Core Update Nag** additionally stops WordPress auto-updating past its current major, so a site doesn't jump to 7.1 unattended and lose ACF's TinyMCE fields

## Installation

1. Upload the `lcp-blog-options` folder to the `/wp-content/plugins/` directory
2. Activate the plugin through the 'Plugins' menu in WordPress
3. Navigate to Tools > LC Blog Options to configure settings

## Usage

### Admin Interface

The plugin adds a "LC Blog Options" page under the WordPress admin Tools menu. This page contains seven checkboxes:

1. **Disable Blog** - When checked, completely disables all blog functionality and automatically enables options 2, 3, and 4
2. **Disable Comments** - When checked, disables all comment-related functionality
3. **Disable Gravatars** - When checked, disables gravatar/avatar display
4. **Disable Tags** - When checked, removes post tags from the admin UI
5. **Disable Emojis** - When checked, removes WordPress emoji assets and TinyMCE emoji support
6. **Suppress Object Cache Warning** - When checked, hides the Site Health "persistent object cache" warning
7. **Suppress Core Update Nag** - When checked, hides the "WordPress x.x is available" admin banner and blocks major core auto-updates

### Checkbox Behavior

- When "Disable Blog" is checked, "Disable Comments", "Disable Gravatars", and "Disable Tags" are automatically checked and cannot be unchecked
- Individual comment, gravatar, tag, and emoji options can be controlled independently when blog is not disabled

## What Gets Disabled

### When Blog is Disabled:

- Posts menu is removed from admin
- All post-related admin pages are inaccessible
- Post type support is disabled
- Blog-related dashboard widgets are removed
- New Post button is removed from admin bar
- All comment functionality (inherited)
- All gravatar functionality (inherited)

### When Comments are Disabled:

- Comments are closed on all posts
- Existing comments are hidden
- Comments menu is removed from admin
- Comment-related dashboard widgets are removed
- Discussion settings page is inaccessible
- Comments section is removed from admin bar

### When Gravatars are Disabled:

- Avatar display is turned off
- Gravatar images are replaced with empty content
- Avatar options are removed from user profiles

### When Tags are Disabled:

- Post tags are detached from the default `post` post type
- The tags admin screen is hidden
- The tags metabox is removed from the post editor

### When Emojis are Disabled:

- Frontend and admin emoji scripts and styles are removed
- TinyMCE emoji support is removed
- The emoji SVG CDN URL is disabled

## Technical Details

- **Version**: 1.2.0
- **Requires**: WordPress 4.0+
- **PHP**: 5.6+
- **License**: GPL v2 or later

## File Structure

```
lcp-blog-options/
├── lc-blog-options.php    # Main plugin file
├── assets/images/lc-full.jpg # Dashboard widget image
└── README.md                # This documentation
```

## Hooks and Filters Used

The plugin uses various WordPress hooks and filters to achieve its functionality:

- `admin_menu` - For adding/removing menu items
- `admin_init` - For settings registration and redirects
- `init` - For disabling post type support
- `wp_dashboard_setup` - For removing dashboard widgets
- `admin_bar_menu` - For modifying admin bar
- `comments_open` / `pings_open` - For disabling comments
- `get_avatar` - For disabling gravatars
- `allow_major_auto_core_updates` - For blocking major core auto-updates
- `enqueue_block_assets` / `enqueue_block_editor_assets` - For ACF/Gutenberg iframe fixes
- `acf/input/admin_enqueue_scripts` - For the QTags crash fix

## Support

This plugin is provided as-is. For customizations or support, please contact the plugin author.

## Changelog

### 1.2.0

- Added a "Suppress Core Update Nag" option that hides the "WordPress x.x is available" admin banner and blocks WordPress from auto-updating to a new major version on its own (minor/security updates unaffected, major-block independently filterable via `lcp_block_major_core_auto_updates`)
- Added WP 7.1 ACF/Gutenberg hardening to deal with the block editor canvas moving into an iframe:
  - Made the ACF edit-mode enforcement filterable via `lcp_acf_blocks_force_edit_mode`
  - Re-enqueue ACF/editor/button styles inside the iframe canvas and tag its body with `wp-core-ui`
  - Hide ACF's duplicate inspector-sidebar panel
  - Fix a WP 7.1 QTags/`buildQuicktags` crash that could break ACF field init
  - Prevent TinyMCE focus-steal/scroll-jump when adding a WYSIWYG field inside an ACF repeater
- Replaced the older "force edit mode on insert" JS watcher with the simpler always-on subscriber, now gated by the filter above

### 1.0.0

- Initial release
- Added blog disable functionality
- Added comments disable functionality
- Added gravatars disable functionality
- Added admin interface under Tools menu

### 1.1.1

- Forced ACF blocks into edit mode immediately when inserted in the block editor
- Fixed the edit-mode enforcement so it also works for newly added blocks in WordPress 7.0

### 1.1.2

- Added the Lamcat dashboard widget to the WordPress admin dashboard
- Moved the dashboard widget image into the plugin at `assets/images/lc-full.jpg`
