---
icon: square-check
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

# Final Services

## Overview

The `FinalService` is a component of the Biruni framework designed to handle post-request processing tasks, such as sending SMS, emails, or notifications. It executes at the end of request and supports both synchronous and asynchronous execution.

{% hint style="warning" %}
**Note**: Synchronous execution will be deprecated in future versions, and asynchronous execution will become the default.
{% endhint %}

## Usage

### Implementing a Final Service

To create a custom FinalService, extend the `uz.greenwhite.biruni.old.service.FinalService` class and override the `run(data: Seq[Any])` method.

The `run` method processes the input data, which is passed as a sequence of arbitrary types (`Seq[Any]`).

### **Steps:**

1. **Create a Service Class**: Annotate your class with `@Service` and extend `FinalService`.
2. **Define Data Handling**: Parse the input `Seq[Any]` into a meaningful structure for your use case.
3. **Implement Logic**: Process the data in the `run` method to perform the desired task (e.g., sending notifications).

### **Example**

The following example implements a `BroadcastService` that sends WebSocket messages to specified user IDs.

```scala
import org.springframework.stereotype.Service
import uz.greenwhite.biruni.old.service.FinalService

@Service
class BroadcastService extends FinalService {
  // Case class to structure the input data
  case class BroadcastMessage(message: String, userIds: Set[Int])

  // Companion object to parse Seq[Any] into BroadcastMessage
  private object BroadcastMessage {
    def apply(s: Any): BroadcastMessage = {
      val x = s.asInstanceOf[Seq[Any]]
      val message = x.head.asInstanceOf[String]
      val userIds = x(1).asInstanceOf[Seq[Any]].map(_.asInstanceOf[String].toInt)
      BroadcastMessage(message, userIds.toSet)
    }
  }

  // Override run method to process data
  override def run(data: Seq[Any]): Unit = {
    val broadcastMessages = data.map(BroadcastMessage(_))
    for {
      m <- broadcastMessages
      id <- m.userIds
    } WebSocketNotifier.broadcast(id, m.message)
  }
}
```

### Invoking Final Service from PL/SQL

To trigger a `FinalService` during request processing, use one of the `Add_Final_Service` PL/SQL procedures. These procedures queue the service for execution.

**Available Procedures**

1. **Using Hashmap**:

```sql
Procedure Add_Final_Service
(
  i_Class_Name varchar2,
  i_Data       Hashmap,
  i_Is_Async   varchar2 := 'N'
);
```

2. **Using Arraylist**:

```sql
Procedure Add_Final_Service
(
  i_Class_Name varchar2,
  i_Data       Arraylist,
  i_Is_Async   varchar2 := 'N'
);
```

* **Parameters**:
  * **i\_Class\_Name**: Fully qualified name of the `FinalService` implementation (e.g., `uz.greenwhite.biruni.old.service.finalservice.SendEmailService`).
  * **i\_Data**: Data to pass to the `run` method, either as a `Hashmap` or `Arraylist`.
  * **i\_Is\_Async**: `'Y'` for asynchronous execution, `'N'` for synchronous execution (synchronous mode will be removed in future versions).

### **Example**

To trigger the `SendEmailService` asynchronously with an `Arraylist` of data:

```plsql
Add_Final_Service(i_Class_Name => 'uz.greenwhite.biruni.old.service.finalservice.SendEmailService',
                  i_Data       => Arraylist('Subject: Welcome', 'user1@example.com'),
                  i_Is_Async   => 'Y');
```

#### Error Logging

If the `run` method of a `FinalService` encounters an error, the error details are logged in the `biruni_final_service_log` table. To view the logs, query the table as follows:

```sql
select *
  from Biruni_Final_Service_Log
 order by Log_Date desc;
```

This query retrieves the most recent error logs first, allowing you to diagnose issues with your `FinalService` execution.

### Deprecation Notice

* **Synchronous execution** (`i_Is_Async = 'N'`) is **deprecated** and will be removed in future versions.
* Plan to migrate all **FinalService** implementations to **asynchronous execution** (`i_Is_Async = 'Y'`).

> **Important:**\
> If you are using synchronous mode (`i_Is_Async = 'N'`), you must override:
>
> ```scala
> def run(request: HttpServletRequest, data: Seq[Any]): Unit
> ```
>
> rather than the usual asynchronous `run(data: Seq[Any])`.

### Common Use Cases

* **Notifications**: Sending SMS, emails, or WebSocket messages after request processing.
* **Cleanup**: Performing post-request cleanup tasks, such as clearing temporary data.

### Best Practices

* **Type Safety**: Carefully parse `Seq[Any]` to avoid runtime errors. Use case classes or companion objects for structured data.
* **Asynchronous Preference**: Favor asynchronous execution (`i_Is_Async = 'Y'`) to improve performance and prepare for future versions.
* **Error Handling**: Implement robust error handling in the `run` method to manage invalid data or external service failures. Check the `biruni_final_service_log` table for debugging.
* **Testing**: Test your `FinalService` implementation with sample data to ensure correct parsing and processing.
