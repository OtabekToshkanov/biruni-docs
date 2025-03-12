---
icon: gem
---

# Features

### 1.  API Management

* **Routing system**: Efficient request handling and navigation across the application. &#x20;
* **Route Redirect:** Allows redirecting requests from old routes to new ones, ensuring smooth transitions and backward compatibility.
* **REST APIs**: Facilitates communication between services.
* **Webhooks**: Supports event-driven interactions with external applications.

### 2. Authentication & Security

Ensuring high levels of security and authentication mechanisms:

* **Licensing:** Enforces access control and software licensing capabilities.
* **Role-based access control**: Restricts access based on user roles and permissions.
* **Session-based web authentication**: Provides secure user sessions for web applications.
* **Token-based mobile authentication**: Enhances mobile application security through tokenization.
* **IP-based restrictions**: Limits system access from specific IP ranges.
* **Concurrent session management**: Monitors and controls simultaneous user sessions.

### 3. Integration

Biruni seamlessly integrates with various external systems and APIs:

* **Telegram**: Allows automated messaging and bot interactions.
* **E-IMZO & OneID**: Provides electronic signature and identity verification.
* **Qlik & Apache Superset**: Supports data visualization and business intelligence solutions.
* **Map**: Seamlessly integrates with mapping services for location-based data.
* **ONLYOFFICE**: supports viewing of the office files (with support for all the popular formats: _DOC, DOCX, XLS, XLSX, CSV, PPTX, ..._) on the browser.

### 4. Reporting & Analytics

Biruni offers powerful analytics and reporting capabilities:

* **Reporting engine**: Generates structured reports efficiently and you can download them as Excel, HTML, PDF.
* **Lazy Reports**: Reports that are generated on demand in a separate thread. This allows users to continue working while the report is generated in the background. Once completed, users are notified and can download the report.
* **Easy Reports**: Generate reports from a data source using a template.

### 5. Internationalization

Biruni is designed for global applications with built-in multilingual support:

* **Multi-language support**: Provides translations for different regions.
* **Dynamic language switching**: Allows seamless transitions between languages within the application.

### 6. Rapid Frontend & Backend Development

Biruni simplifies the development of frontend forms and backend logic:

* **Pre-built frontend components**: Utilize components like **b-grid, b-grid-controller, b-input** and more to quickly create UI forms.
* **Hotkeys in UI forms:** Enhance productivity with keyboard shortcuts for faster navigation and data entry using **b-hotkey.**
* **Backend utility tools**: Efficiently manage data using **Fazo\_Query**, **HashMap**, and other tools within the Oracle Database.
* **Seamless integration**: Ensures smooth communication between frontend and backend, reducing development effort.

### 7. Notification/Messaging

* **WebSocket**: Enables real-time notifications to users, ensuring instant updates and seamless communication
* **Messaging Services:** Supports sending messages via SMS and email.
* **Push notifications:** Facilitates push notifications for Android, Huawei and iOS devices via Firebase Cloud Messaging (FCM).

### 8. Other

* **File management**: Supports storing files either in **Oracle Database** or **S3 object storage**.
* **Background job processing**: Enables scheduling and executing long-running tasks asynchronously.
* **Standby database:** If configured, specific routes can be designated to use the standby database (e.g., for read requests), reducing the load on the primary database and improving performance
* **Barcode & QR Code Generation:** Supports generating barcodes, QR codes, and GS1 DataMatrix in image and byte array formats.
