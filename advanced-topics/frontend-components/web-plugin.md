---
icon: plug
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Web-Plugin

## Usage

1. Create a JavaScript plugin file in the directory: `page/resource/{project_code}/`.
2. Implement the plugin logic inside the `onSessionResolve.then()` block in the created file.
3. Update `md_projects.web_plugin_file` with the name of the created file.

## Example:

1. Created plugin.js file in directory `verifix\main\page\resource\vhr`.
2.  Implemented plugin logic inside the promise block.

    ```javascript
    window.onSessionResolve.then(ui => {
      /*
        ui argument includes properties below:
        company_code, company_name, filial_id, filial_name,
        user_id, name, photo_sha, is_admin, lang_code
      */
      // plugin logic goes here...
    });
    ```
3. Inserted `plugin.js` to md\_projects.web\_plugin\_file.

<figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/plugin-js.png" alt=""><figcaption></figcaption></figure>
