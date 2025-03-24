---
icon: file-spreadsheet
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

# Easy report

Easy Report is a customizable, user-friendly, template-based reporting tool. Users can define their own report templates in Excel using specified keywords, allowing for personalized report generation based on the provided template.

Easy report consists of three main components as shown below:

<figure><img src="../../../.gitbook/assets/easy_report.jpg" alt=""><figcaption></figcaption></figure>

Both **"Data"** and **"Definitions"** are written in PL/SQL within UI packages. In contrast, the **"Template"** is an Excel document that utilizes keywords from **"Definitions"**. During report generation, these keywords are replaced with corresponding **"Data"** values.

## Definitions

It is necessary to first define the definitions based on business logic. A **definition** is a part of Easy Report that contains similar map objects that have `key`, `definition` and `items` fields:

* **key** — The map key used for integration with actual data.
* **definition** — The name or description of the current item.
* **items** — (Optional) Used when the object has child definitions, each with its own `key`, `definition`, and `items`.

```json
[
    // example without items field
    {
        "key": test_key,
        "definition": Test Key
    },
    // example with items field
    {
        "key": test_items_key,
        "definition": Test Items Key
        "items": [
            {
                "key": test_child_key,
                "definition": Test Child Key
            }
        ]   
    }
]
```

{% hint style="info" %}
The definition's hierarchical structure can reach any depth via `items` field.
{% endhint %}

The Definition fucntion should return `Arraylist`:

```plsql
Function Definitions return Arraylist is
    /*
       variables
    */
    result Arraylist := Arraylist;
begin
    /*
       inserting definition item into the result
    */
    return result;
end;
```

### Inserting item into definition

Since **"Definitions"** is an array of similar map objects, each map object can be inserted using following procedure:

```plsql
Procedure Put
(
    p_List       in out nocopy Arraylist,
    i_Key        varchar2,
    i_Definition varchar2,
    i_Items      Arraylist := null
) is
    v_Map Hashmap := Hashmap;
begin
    v_Map := Fazo.Zip_Map('key', i_Key, 'definition', i_Definition);

    if i_Items is not null then
        v_Map.Put('items', i_Items);
    end if;
    
    p_List.Push(v_Map);
end;
```

Additionally, child items can also use this procedure since they follow the same structure.

## Data

We have created definition keywords based on business logic; now, we must also provide data for each keyword. Otherwise, creating such definition keywords would be pointless.

The root data object is `Gmap` data type and it contains values attached to each definition keyword, for example:



## Template

* vertical horizonal loops
* page breakables

## Usage

### Binding template

## Code structure

## Only Office
