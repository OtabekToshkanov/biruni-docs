---
icon: calendar-clock
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

# Lazy report

Lazy report is a report that is generated on demand in separate thread. It is used to generate reports that take a long time to generate. Lazy report is generated in background and user can continue to work with the application while report is being generated. When report is generated, user is notified about it and can download it.

## Usage

To use lazy report, the report generation function must return a `Lazy_Report` type. Simple report generation stays the same; the only difference is returning a `Lazy_Report` type.

```plsql
Function Run(p Hashmap) return Lazy_Report is
begin
  /*
    old report generation code
  */
  return Lazy_Report;
end;
```

## **Process Flow**

1. When a user requests a Lazy Report, a new thread is created to execute the report generation function.
2. The function returns a `Lazy_Report` type, which contains a unique ID to identify the report.
3. This ID is used to check the report's status and download it once ready.

## **Report Statuses**

A Lazy Report can have one of the following four statuses:

* **NEW** — The report request has been created but has not started execution.
* **EXECUTING** — The report is currently being generated.
* **COMPLETED** — The report has been successfully generated and is ready for download.
* **FAILED** — The report generation has failed.

## **Data Storage**

* Lazy Report data is stored in the **`biruni_lazy_report_register`** table.
* Additional reference data is stored in the **`md_lazy_report_register`** table.
* You can check the report status directly from these tables.

## **Managing Reports**

Lazy reports can be managed from the Lazy report list page (`/biruni/md/lazy_report_list`). This page provides an overview of all reports and their statuses, allowing you to track, download, or delete them as needed.

## **Waiting and Notifications**

* By default, the user waits up to **60 seconds** for the report to generate.
* If the report is completed within this time, it is downloaded instantly.
* If the report is not ready within **60 seconds**, the user is notified that it is being generated in the background. The user can continue working within the application.
* Once the report is generated, the user receives a notification and can download it.

## **Limitations**

* A user can generate only **one** Lazy Report at a time for each report type.
* If a user tries to generate another Lazy Report while one is still in progress, they are notified that another report is being generated. The user can continue working without interruption.



