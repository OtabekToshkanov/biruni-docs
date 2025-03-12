---
description: >-
  This document outlines the system requirements for setting up and running
  applications built on the Biruni framework, including necessary software
  dependencies.
icon: diamonds-4
---

# System Requirements

### Database Requirements

* **Oracle Database (v19)**

### Application Server Requirements

The following tools must be installed on the same machine where you intend to run the application, depending on the deployment environment(Maven, Tomcat):

{% tabs %}
{% tab title="Maven" %}
* **Java Development Kit (v21)**
* **Maven (v3 or above)**
* **Node.js (v20) with npm (v11)**
{% endtab %}

{% tab title="Tomcat" %}
* **Java Development Kit (v21)**
* **Apache Tomcat (v10)**
{% endtab %}
{% endtabs %}
