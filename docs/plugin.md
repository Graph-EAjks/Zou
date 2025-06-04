# Kitsu API plugin system

Kitsu API (Zou) plugin system allows you to create modular extensions that integrate directly into the API. Each plugin includes a `manifest.toml` file with metadata and can contain database migrations, Flask blueprints, and more.

---

## Plugin CLI Commands

### `install_plugin`

Install a plugin from a local folder or `.zip` package.

```bash
zou install-plugin --path /path/to/plugin
```

**Options:**

* `--path`: Path to the plugin folder or `.zip` archive (required).
* `--force`: Overwrite if already installed (default: `False`).

**Note:** Restart the server after installing a plugin.

---

### `uninstall_plugin`

Uninstall a previously installed plugin.

```bash
zou uninstall-plugin --id my_plugin
```

**Options:**

* `--id`: Plugin ID to uninstall (required).

**Note:** This removes plugin files and unregisters its migrations.

---

### `create_plugin_skeleton`

Generate the basic structure of a new plugin.

```bash
zou create-plugin-skeleton --path ./plugins --id my_plugin
```

**Options:**

* `--path`: Directory where the plugin will be created (required).
* `--id`: Unique plugin ID (required).
* `--name`: Human-readable plugin name (default: `"MyPlugin"`).
* `--description`: Short description (default: `"My plugin description."`).
* `--version`: Semantic version string (default: `"0.1.0"`).
* `--maintainer`: Contact info (default: `"Author <author@author.com>"`).
* `--website`: Plugin/project website (default: `"mywebsite.com"`).
* `--license`: License (default: `"GPL-3.0-only"`).
* `--force`: Overwrite if directory exists (default: `False`).

This command generates:

* `manifest.toml` with plugin metadata
* Initial folder structure for code, migrations, etc.

---

### `create_plugin_package`

Package a plugin folder into a zip file, ready for installation.

```bash
zou create-plugin-package --path ./plugins/my_plugin --output-path ./dist
```

**Options:**

* `--path`: Plugin source folder (required).
* `--output-path`: Destination folder for the `.zip` (required).
* `--force`: Overwrite existing archive if present (default: `False`).

---

### `list_plugins`

List all currently installed plugins.

```bash
zou list-plugins
```

**Options:**

* `--format`: `table` or `json` (default: `table`).
* `--verbose`: Show more metadata.
* `--filter-field`: Filter by a specific field (`plugin_id`, `name`, `maintainer`, `license`).
* `--filter-value`: Value to search in the selected field.

---

### `migrate_plugin_db`

Run database migrations for a plugin.

```bash
zou migrate-plugin-db --path ./plugins/my_plugin
```

**Options:**

* `--path`: Path to the plugin folder (required).
* `--message`: Optional migration message (default: `""`).

This generates and applies Alembic migration scripts for the plugin’s database schema.

---

## Plugin structure

A typical plugin created with `create_plugin_skeleton` looks like this:

```
my_plugin/
├── manifest.toml
├── migrations/
│   ├── env.py
│   ├── versions/
├── my_plugin/
│   ├── __init__.py
│   └── views.py
```

### `manifest.toml`

Contains the plugin metadata:

```toml
id = "my_plugin"
name = "My Plugin"
version = "0.1.0"
description = "My plugin description."
maintainer = "Author <author@example.com>"
website = "mywebsite.com"
license = "GPL-3.0-only"
```

---

## Best practices

* Use unique plugin IDs.
* Follow semantic versioning (`x.y.z`).
* Include at least one route or feature inside your plugin module.
* Write migrations if your plugin defines database models.
* For license use SPDX identifier (see [here](https://spdx.org/licenses/)). 

---

## Developer workflow

1. Create plugin:
    ```
    zou create-plugin-skeleton --path ./plugins --id my_plugin
    ```
2. Implement logic in `views.py`.
3. Add DB models and generate migrations:
    ```
    zou migrate-plugin-db --path ./plugins/my_plugin
    ```
4. Package it:
    ```
    zou create-plugin-package --path ./plugins/my_plugin --output-path ./dist
    ```
5. Install it:
    ```
    zou install-plugin --path ./dist/my_plugin.zip
    ```
6. List installed plugins:
    ```
    zou list-plugins
    ```
7. Uninstall if needed:
    ```
    zou uninstall-plugin --id my_plugin
    ```

---
