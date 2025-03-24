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

Definition keywords have been created based on business logic, and data must now be provided for each keyword. Without this, the creation of such definition keywords would serve no purpose.

The main data object is `Gmap` data type and contains values associated with each definition keyword. Three types of data can be attached to the keywords: scalar, list, image.

### Scalar data

PL/SQL scalar data types include VARCHAR2, BOOLEAN, NUMBER, DATE, and others. These data types can be easily assigned using the `put` member method of the `Gmap` type, as shown below:

<pre class="language-plsql"><code class="lang-plsql">---- declare
<strong>v_data Gmap := Gmap;
</strong>---- body
v_data.put('test_varchar2', 'test_data'); -- varchar2
v_data.put('test_number', 10);            -- number
v_data.put('test_boolean', true);         -- boolean
v_data.put('test_date', sysdate);         -- date
</code></pre>

### List data

If a definitions item contains an `items` field, it should be treated as list data. Each entry within the items field is itself a definition, meaning it is also `Gmap` data type.

List consists of `Gmap` objects, and this list itself represented by the `Glist` data type.

```plsql
---- declare
v_data      Gmap := Gmap;
v_list      Glist := Glist;
v_list_item Gmap;
---- body
-- first list item
v_list_item := Gmap;
v_list_item.put('item_1_1', 'test_data');
v_list_item.put('item_1_2', 'test_data');
v_list_item.put('item_1_3', 'test_data');
v_list.push(v_list_item.Val);
-- second list item
v_list_item := Gmap;
v_list_item.put('item_2_1', 'test_data');
v_list_item.put('item_2_2', 'test_data');
v_list.push(v_list_item.Val);

data.put('test_list', v_list);
```

### Image data

Easy Report supports three types of image data:

* photo
* barcode
* qrcode

To use image data, a `Gmap` object must be assigned to a keyword with specific fields.&#x20;

```plsql
---- declare
v_data    Gmap := Gmap;
v_photo   Gmap := Gmap;
v_barcode Gmap := Gmap;
v_qrcode  Gmap := Gmap;
---- begin
-- photo
v_photo.put('type', 'photo');          -- required for photo
v_photo.put('value', /* sha256 */);    -- sha hashcode of image
-- barcode
v_barcode.put('type', 'barcode');      -- required for barcode image
v_barcode.put('value', /* input */);   -- input source generated into barcode
v_barcode.put('width', /* number */);  -- width of the barcode image
v_barcode.put('height', /* number */); -- height of the barcode image
-- qrcode
v_barcode.put('type', 'qrcode');       -- required for qrcodeimage
v_barcode.put('value', /* input */);   -- input source generated into qrcode
v_barcode.put('width', /* number */);  -- width of the qrcode image
v_barcode.put('height', /* number */); -- height of the qrcode image

v_data.put('test_photo', v_photo);
v_data.put('test_barcode', v_barcode);
v_data.put('test_qrcode',, v_qrcode);
```

The default width and height for all image types are set to `200`.

## Template

* vertical horizonal loops
* page breakables

## Usage

### Binding template

## Code structure

## Only Office
