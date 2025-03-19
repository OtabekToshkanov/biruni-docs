# Biruni report

## Report

### Creating report

Reports are generated using the `b_report` package. There are two ways to open a workbook:

#### Standard Workbook:

```plsql
b_report.Open_Book(report_type, file_name);
```

#### Workbook with Styles:

```plsql
b_report.Open_Book_With_Styles(report_type, file_name);
```

Once the workbook is opened with styles, it functions as a standard workbook but includes additional [built-in styles](biruni-report.md#built-in-styles).

#### Parameters

* `report_type` – Specifies the [type of report](biruni-report.md#report-types) to generate.
* `file_name` – The name of the output file.

Both functions have their overloaded versions where the first parameter can be a `HashMap`. In this case, the map must contain a key `rt`, with its value specifying the report type.

After opening report it should be closed at the end using `b_report.Close_Book`&#x20;

{% hint style="warning" %}
There cannot be opened more than one workbook at a time
{% endhint %}

### Report hierarchy

Biruni's report structure is organized into a hierarchical format, ensuring clear data representation and styling control:

* **b\_report** — The root container that holds one or more `b_table`.
* **b\_table** — Represents a table within the report, similar to sheets in Excel, and contains multiple rows.
* **Row** — A collection of cells
* **Cell** — The smallest unit where data is written and styles are applied.

<figure><img src="../../../.gitbook/assets/b_report_hierarchy.png" alt=""><figcaption></figcaption></figure>

### Report types

<table><thead><tr><th width="160">Output</th><th width="122" align="center">Report type</th><th width="320">Description</th><th align="center">Extension</th></tr></thead><tbody><tr><td>Excel</td><td align="center">xlsx</td><td></td><td align="center">.xlsx</td></tr><tr><td>Html</td><td align="center">html</td><td></td><td align="center">.html</td></tr><tr><td>Html Mobile</td><td align="center">htmlm</td><td>Responsive html code for mobile</td><td align="center">.html</td></tr><tr><td>Html Style</td><td align="center">htmls</td><td></td><td align="center">.html</td></tr><tr><td>Html Debug</td><td align="center">htmld</td><td></td><td align="center"></td></tr><tr><td>Html JOD</td><td align="center">htmljod</td><td>Converted from excel report using JODConverter</td><td align="center">.html</td></tr><tr><td>PDF</td><td align="center">pdf</td><td>Converted from excel report using JODConverter</td><td align="center">.pdf</td></tr><tr><td>CSV</td><td align="center">csv</td><td>Comma Separated Value(s)</td><td align="center">.csv</td></tr><tr><td>XML</td><td align="center">xml</td><td></td><td align="center">.xml</td></tr></tbody></table>

## Tables

### Creating table

Creating rows

Creating cells

## Data



## Styles

Biruni provides a solid styling system for workbooks, supporting colors, fonts, borders, and more to enhance report appearance while offering flexible customization.

#### Font styles

<table><thead><tr><th width="149">Font style</th><th width="408">Description</th><th>Sample input</th></tr></thead><tbody><tr><td>Font size</td><td>Accepts positive numbers, sets font size of the text.</td><td>20</td></tr><tr><td>Font color</td><td>Accepts <code>varchar2</code> and supports hex color codes and named colors.</td><td><p>hex: <code>#FFCCBB</code></p><p>named: <code>aqua</code></p></td></tr><tr><td>Font family</td><td>Accepts <code>varchar2</code> and supports font families supported in Excel and CSS.</td><td><code>sans-serif</code></td></tr><tr><td>Font bold</td><td><code>boolean</code> data type, makes text bold.</td><td>true</td></tr><tr><td>Font Italic</td><td><code>boolean</code> data type, makes text italic.</td><td>true</td></tr><tr><td>Font underline</td><td><code>boolean</code> data type, makes text underlined.</td><td>true</td></tr></tbody></table>

#### Borders

<table><thead><tr><th width="176">Border</th><th width="379">Description</th><th>Sample input</th></tr></thead><tbody><tr><td>Border</td><td>Accepts <code>Option_Varchar2</code> and applies a border to all four sides of the cell.</td><td><code>b_report.b_Double</code></td></tr><tr><td><p>Border top</p><p>Border bottom</p><p>Border left</p><p>Border right</p></td><td>Accepts <code>Option_Varchar2</code> and applies a border to specified side of the cell.</td><td><code>b_report.b_Dashed</code></td></tr><tr><td>Border color</td><td>Accepts <code>varchar2</code> and supports hex color codes and named colors.</td><td><p>hex: <code>#333BBB</code></p><p>named: <code>magenta</code></p></td></tr><tr><td><p>Border top color</p><p>Border bottom color</p><p>Border left color</p><p>Border right color</p></td><td>Accepts <code>varchar2</code> and supports hex color codes and named colors.</td><td><p>hex: <code>#ABCDEF</code></p><p>named: <code>yellow</code></p></td></tr></tbody></table>

There are constants for the border values in `biruni_report`:

* constaints: `b_Null`, `b_Hair`, `b_Thin`, `b_Thick`, `b_Double`, `b_Dotted`, `b_Dashed`, `b_Dash_Dot`, `b_Dash_Dot_Dot`, `b_Medium`, `b_Medium_Dashed`, `b_Medium_Dash_Dot`, `b_Medium_Dot_Dot`.
* data type: `Option_Varchar2`&#x20;
* usage: `b_report.border_type` (e.g. `b_report.b_Medium_Dashed`)

#### Text alignments

* Horizontal align
* Vertical align

#### Other formatting options

<table><thead><tr><th width="186">Formatting options</th><th width="302">Description</th><th>Sample input</th></tr></thead><tbody><tr><td>Text rotate</td><td>Takes a <code>number</code> and supports a range of -90 to 90 for Excel and -180 to 180 for HTML.</td><td>-10</td></tr><tr><td>Text wrap</td><td><code>boolean</code> data type that, when enabled, wraps text to a new line if it exceeds the cell's width.</td><td>true</td></tr><tr><td>Shrink to fit</td><td><code>boolean</code> data type that, when enabled, reduces text size to fit within the cell dimensions. Works only in Excel.</td><td>true</td></tr><tr><td>Indent</td><td>Takes a <code>number</code> and adds leading space to shift the text to the right within the cell.</td><td>3</td></tr><tr><td>Cell format</td><td>Accepts <code>varchar2</code> and follow Excel's formatting syntax. It can be interprated as number, percentage, currency, data and more. Works only in Excel.</td><td><code>$#,#0.00</code></td></tr><tr><td>Background color</td><td>Accepts <code>varchar2</code> and supports hex color codes and named colors.</td><td><p>hex: <code>#FEDCBA</code></p><p>named: <code>green</code></p></td></tr></tbody></table>



```plsql
    i_Font_Size           number := null,
    i_Font_Color          varchar2 := null,
    i_Font_Family         varchar2 := null,
    i_Font_Bold           boolean := null,
    i_Font_Italic         boolean := null,
    i_Font_Underline      boolean := null,
    
    i_Horizontal_Align    Option_Number := null,
    i_Vertical_Align      Option_Number := null,
    
    i_Text_Rotate         number := null,
    i_Text_Wrap           boolean := null,
    i_Shrink_To_Fit       boolean := null, // only Excel
    i_Indent              number := null,
    i_Background_Color    varchar2 := null,
    i_Cell_Format         varchar2 := null,
    
    i_Border              Option_Varchar2 := null,
    i_Border_Color        varchar2 := null,
    i_Border_Top          Option_Varchar2 := null,
    i_Border_Top_Color    varchar2 := null,
    i_Border_Bottom       Option_Varchar2 := null,
    i_Border_Bottom_Color varchar2 := null,
    i_Border_Left         Option_Varchar2 := null,
    i_Border_Left_Color   varchar2 := null,
    i_Border_Right        Option_Varchar2 := null,
    i_Border_Right_Color  varchar2 := null
```

### Creating styles

### Usage

### Built-in styles

