---
icon: file-chart-pie
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
    visible: false
---

# Reports

Currently in biruni there are three type of reports can be generated:

### Biruni report

Biruni Report is a structured reporting system designed for organizing and formatting data in a hierarchical manner. It allows sequential data insertion, flexible customization of layout and styles, and supports various content types like text, tables, and images. With built-in and custom styling options, it ensures clear data representation and professional report generation.

{% content-ref url="biruni-report.md" %}
[biruni-report.md](biruni-report.md)
{% endcontent-ref %}

***

### Lazy report

A Lazy Report is a background-generated report designed for long-running tasks, allowing users to continue working while the report is being processed. It runs in a separate thread, tracks progress with unique statuses, and notifies users when ready for download. This system ensures efficient report management without interrupting workflow.

{% content-ref url="lazy-report.md" %}
[lazy-report.md](lazy-report.md)
{% endcontent-ref %}

***

### Easy report

Easy Report is a flexible, template-based reporting tool that allows users to define custom report templates in Excel using predefined keywords. It integrates with backend business logic to dynamically replace template placeholders with corresponding data, including text, lists, and images. With built-in support for loops, structured data, and page breaks, Easy Report simplifies complex report generation while ensuring customization and consistency.

{% content-ref url="easy-report.md" %}
[easy-report.md](easy-report.md)
{% endcontent-ref %}



