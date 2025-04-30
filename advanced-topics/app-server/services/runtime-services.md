---
icon: person-running
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

# Runtime Services

## Overview

The `RuntimeService` is a component of the Biruni framework designed to handle tasks that are not efficiently performed in Oracle PL/SQL, such as sending HTTP requests, performing complex computations, or integrating with external systems. It is invoked during request processing, executes in a Java/Scala environment, and returns results to a specified PL/SQL response procedure. The entire process, including the initial PL/SQL call, Java/Scala execution, and subsequent Oracle processes (e.g., review and response procedures), occurs within a single database transaction.

## Usage

### Implementing a Runtime Service

To create a custom `RuntimeService`, extend the `uz.greenwhite.biruni.old.service.RuntimeService` class and override the `run(detail: Map[String, Any], data: String): RuntimeResult` method.

The `run` method processes input data, where:

* `detail`: A map containing metadata about the request (e.g., company ID, instance code).
* `data`: A string (typically JSON) containing the payload to process.

The method returns a `RuntimeResult`, which can be either:

* `SuccessRuntimeResult`: Contains the successful result (e.g., a JSON string).
* `ErrorRuntimeResult`: Contains an error message if an exception occurs.

### **Steps:**

1. **Create a Service Class**: Annotate your class with `@Service` and extend `RuntimeService`.
2. **Parse Input Data**: Parse the `data` string (e.g., JSON) and use `detail` for context.
3. **Implement Logic**: Process the data in the `run` method and return a `RuntimeResult`.

### **Example**

The following example implements a `BiruniCryptoService` that encrypts a secret key using RSA encryption.

```scala
import org.springframework.stereotype.Service
import uz.greenwhite.biruni.old.service.RuntimeService
import uz.greenwhite.biruni.old.service.RuntimeResult
import uz.greenwhite.biruni.util.JSON

@Service
class BiruniCryptoService extends RuntimeService {

  override def run(detail: Map[String, Any], data: String): RuntimeResult = {
    try {
      var m: Map[String, Any] = JSON.parseForce(data)
      m += "secret_key" -> DigitalSignatureRSA.encrypt(
        m("secret_key").asInstanceOf[String],
        m("rsa_public_key").asInstanceOf[String]
      )
      SuccessRuntimeResult(JSON.stringify(m))
    } catch {
      case ex: Exception => ErrorRuntimeResult(ex.getMessage)
    }
  }
}
```

### Invoking Runtime Service from PL/SQL

To trigger a `RuntimeService`, create a `Runtime_Service` instance within a **routed** PL/SQL procedure that returns a `Runtime_Service` object. Configure the service with details, data, and procedures, and return it for execution. The entire process, from the initial PL/SQL call to the Java/Scala execution and subsequent Oracle procedures, is executed within a single database transaction.

### **Example Routed Procedure**

The following PL/SQL function creates and configures a `RuntimeService` for `BillingIntegrationService`:

```sql
Function My_Routed_Procedure return Runtime_Service is
  v_Service Runtime_Service;
  v_Details Hashmap := Hashmap();
  v_Data    varchar2(4000) := '{"key":"value"}';
begin
  -- Initialize the RuntimeService with the class name
  v_Service := Runtime_Service('uz.greenwhite.biruni.old.service.runtimeservice.BillingIntegrationService');

  -- Set metadata (detail)
  v_Details.Put('company_id', '123');
  v_Details.Put('instance_code', 'INST_001');
  v_Service.Set_Detail(v_Details);

  -- Set data (e.g., JSON payload)
  v_Service.Set_Data(v_Data);

  -- Set review procedure (always executed, even on error)
  v_Service.Set_Review_Procedure('Kl_Core.Billing_Review_Procedure');

  -- Set response procedure and action modes
  v_Service.Set_Response_Procedure(Response_Procedure => 'Ui_Biruni177.Response',
                                   Action_In          => 'M',
                                   Action_Out         => 'M');

  return v_Service;
end;
```

* **Key Methods**:
  * **Runtime\_Service(i\_Class\_Name)**: Initializes the service with the fully qualified class name (e.g., `uz.greenwhite.biruni.old.service.runtimeservice.BillingIntegrationService`).
  * **Set\_Detail(i\_Details)**: Sets metadata as a `Hashmap` passed to the `detail` parameter of the `run` method.
  * **Set\_Data(i\_Data)**: Sets the payload (e.g., JSON string) passed to the `data` parameter of the `run` method.
  * **Set\_Review\_Procedure(i\_Procedure)**: Specifies a PL/SQL procedure to review the service execution (e.g., `Kl_Core.Billing_Review_Procedure`). This procedure is always executed, even if the `run` method fails.
  * **Set\_Response\_Procedure(Response\_Procedure, Action\_In, Action\_Out)**: Specifies the PL/SQL procedure to handle the response, along with input and output action modes.

The response from the `run` method is passed to the specified `Response_Procedure`. The `Review_Procedure` is executed regardless of whether the `run` method succeeds or fails, allowing for consistent post-execution processing.

### Review Procedure

The `Review_Procedure` is a PL/SQL procedure that is always executed after the `run` method, whether it succeeds or fails. It is typically used for tasks like logging, saving results, or performing cleanup. The procedure receives input (e.g., a JSON string) that can be parsed and processed.

### **Example Review Procedure**

The following example shows a `Billing_Review_Procedure` that saves access and refresh tokens:

```sql
Procedure Billing_Review_Procedure(i_Input varchar2) is
  pragma autonomous_transaction;
  v_Map Hashmap := Fazo.Parse_Map(i_Input);
begin
  z_Kauth_Api_Client_Tokens.Save_One(i_Company_Id    => Md_Env.Company_Id,
                                     i_Code          => Kl_Pref.c_Billing_Cabinet_Code,
                                     i_Access_Token  => v_Map.r_Varchar2('access_token'),
                                     i_Refresh_Token => v_Map.r_Varchar2('refresh_token'));
  commit;
exception
  when others then
    null;
end;
```

* **Notes**:
  * The `PRAGMA AUTONOMOUS_TRANSACTION` allows the procedure to commit independently of the main transaction, which is useful for logging or saving data that should persist even if the main transaction rolls back.
  * The procedure includes basic error handling to prevent unhandled exceptions from affecting the main transaction.

### Transactional Behavior

The entire lifecycle of a `RuntimeService` execution is wrapped in a single database transaction, including:

* The initial PL/SQL call to create and configure the `Runtime_Service` in the routed procedure.
* The Java/Scala execution of the `run` method.
* The execution of the `Review_Procedure` and `Response_Procedure` in PL/SQL.

If any part of the process fails (e.g., an exception in the `run` method), the main transaction is rolled back, ensuring data consistency. However, the `Review_Procedure` may use `PRAGMA AUTONOMOUS_TRANSACTION` to commit its changes independently.

### Error Handling

If the `run` method encounters an error, it returns an `ErrorRuntimeResult` with the exception message. The `Review_Procedure` is still executed, allowing for error logging or cleanup. Implement robust error handling in the `run` method and the `Review_Procedure` to manage invalid data or external service failures. Errors should also be handled within the `Response_Procedure` as configured.

### Common Use Cases

* **External Requests**: Sending HTTP requests to external APIs.
* **Complex Computations**: Performing tasks like encryption, data transformation, or calculations not suited for PL/SQL.
* **Integration**: Interfacing with external systems or services, such as billing or authentication services.

### Best Practices

* **Data Parsing**: Safely parse the `data` string (e.g., JSON) in the `run` method and the `i_Input` parameter in the `Review_Procedure` to avoid runtime errors.
* **Error Handling**: Use `try-catch` blocks in the `run` method to return `ErrorRuntimeResult` for errors. Ensure the `Review_Procedure` and `Response_Procedure` handle error cases gracefully.
* **Testing**: Test your `RuntimeService` and routed procedure with sample `detail`, `data`, and `i_Input` values, including error scenarios, to ensure correct processing and response handling.
* **Metadata Usage**: Leverage the `detail` map for contextual information (e.g., company ID, instance code) to make the service reusable across different scenarios.
* **Procedure Configuration**: Ensure the `Review_Procedure` and `Response_Procedure` are correctly configured to handle both successful and failed executions. Use `PRAGMA AUTONOMOUS_TRANSACTION` in the `Review_Procedure` only when necessary (e.g., for independent commits).
* **Transactional Awareness**: Design your service and procedures with the understanding that all operations occur within a single transaction, except for autonomous transactions in the `Review_Procedure`. Avoid external operations that cannot be rolled back.
