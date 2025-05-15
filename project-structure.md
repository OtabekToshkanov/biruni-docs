---
icon: folder-tree
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

# Project Structure

The Biruni Project is organized into three primary layers:

1. **Frontend**: Developed using AngularJS.
2. **Application Server**: Built with Spring, it acts as a gateway between the frontend and the backend, providing various utility services such as report generation, integration, notifications, and file processing.
3. **Backend and Database**: Implemented in Oracle using PL/SQL, housing most of the project's architecture and framework logic.

<figure><img src=".gitbook/assets/project-structure/framework-layers.png" alt=""><figcaption><p>Biruni Framework Layers</p></figcaption></figure>

The source code for each layer is located in the `main` folder

<figure><img src=".gitbook/assets/project-structure/project-structure.png" alt=""><figcaption><p>Codebase Directory Structure</p></figcaption></figure>
