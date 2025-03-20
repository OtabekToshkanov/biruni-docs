---
icon: table-cells
---

# Biruni report

## Report

### Creating report

Reports are generated using the `b_report` package. There are two ways to open a workbook:

#### Standard Workbook:

```plsql
b_Report.Open_Book(report_type, file_name);
```

#### Workbook with Styles:

```plsql
b_Report.Open_Book_With_Styles(report_type, file_name);
```

Once the workbook is opened with styles, it functions as a standard workbook but includes additional [built-in styles](biruni-report.md#built-in-styles).

#### Parameters

* `report_type` – Specifies the [type of report](biruni-report.md#report-types) to generate.
* `file_name` – The name of the output file.

Both functions have their overloaded versions where the first parameter can be a `HashMap`. In this case, the map must contain a key `rt`, with its value specifying the report type.

After opening report it should be closed at the end using `b_Report.Close_Book`&#x20;

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

<table><thead><tr><th width="160">Output</th><th width="122" align="center">Report type</th><th width="320">Description</th><th align="center">Extension</th></tr></thead><tbody><tr><td>Excel</td><td align="center">xlsx</td><td>Report data is displayed across table rows and columns.</td><td align="center">.xlsx</td></tr><tr><td>Html</td><td align="center">html</td><td>An HTML document where data is displayed using the <code>&#x3C;table></code> tag, with multiple sheets organized into navigation tabs (<code>&#x3C;ul></code> with <code>&#x3C;li></code>). Each tab functions as a separate sheet.</td><td align="center">.html</td></tr><tr><td>Html Mobile</td><td align="center">htmlm</td><td>Responsive html code for mobile devices</td><td align="center">.html</td></tr><tr><td>Html Style</td><td align="center">htmls</td><td></td><td align="center">.html</td></tr><tr><td>Html Debug</td><td align="center">htmld</td><td>Html used for Debug</td><td align="center">.html</td></tr><tr><td>Html JOD</td><td align="center">htmljod</td><td>Converted from excel report using JODConverter</td><td align="center">.html</td></tr><tr><td>PDF</td><td align="center">pdf</td><td>Converted from excel report using JODConverter</td><td align="center">.pdf</td></tr><tr><td>CSV</td><td align="center">csv</td><td>Comma Separated Value(s)</td><td align="center">.csv</td></tr><tr><td>XML</td><td align="center">xml</td><td></td><td align="center">.xml</td></tr></tbody></table>

## Tables

The Biruni reports core component is the table. A table can be considered a spreadsheet that organizes data into rows and columns while also providing various customization options for formatting, layout, and printing.

### Creating table

Tables are created using the `b_table` object type. Since it is an object type, we instantiate it through `b_report` to define the table:

```plsql
test_table b_Table := b_Report.New_Table();
-- or
test_table b_Table := b_Report.New_Table(i_Parent => parent_table);
```

{% hint style="info" %}
When a table is created using a parent parameter, the child table inherits the current style of its parent.
{% endhint %}



Creating rows

Creating cells

## Data

TODO

## Styles

Biruni provides a solid styling system for workbooks, supporting colors, fonts, borders, and more to enhance report appearance while offering flexible customization.

### Constants

Constants play a crucial role in `biruni_report` by ensuring consistency, reusability, and maintainability. They allow predefined values for styles, formatting, and layout to be stored and referenced throughout the report generation process.&#x20;

#### Constants for the border:

* constants: `b_Null`, `b_Hair`, `b_Thin`, `b_Thick`, `b_Double`, `b_Dotted`, `b_Dashed`, `b_Dash_Dot`, `b_Dash_Dot_Dot`, `b_Medium`, `b_Medium_Dashed`, `b_Medium_Dash_Dot`, `b_Medium_Dot_Dot`.
* data type: `Option_Varchar2`&#x20;
* usage: `b_Report.border_type` (e.g. `b_Report.b_Medium_Dashed`)

#### Constants for the text alighnment:

* constants: a\_Left, a\_Center, a\_Right, a\_Top, a\_Middle, `a_Bottom`.
* data type: `Option_Number`&#x20;
* usage: `b_Report.text_alignment` (e.g. `b_Report.a_Middle`)

### Style parameters

#### **Core parameters**

<table><thead><tr><th width="189">Parameter</th><th width="376">Description</th><th>Sample input</th></tr></thead><tbody><tr><td>i_Style_Name</td><td>Accepts <code>varchar2</code>, assigns a unique name to the style, and allows it to be referenced later using this name.</td><td><code>test_style</code></td></tr><tr><td>i_Parent_Style_Name</td><td>Accepts <code>varchar2</code> and inherits properties from the specified parent style.</td><td><code>test_parent_style</code></td></tr></tbody></table>

#### Font styles

<table><thead><tr><th width="149">Parameter</th><th width="432">Description</th><th>Sample input</th></tr></thead><tbody><tr><td>i_Font_Size</td><td>Accepts positive numbers, sets font size of the text.</td><td><code>20</code></td></tr><tr><td>i_Font_Color</td><td>Accepts <code>varchar2</code> and supports hex color codes and named colors.</td><td><p>hex: <code>#FFCCBB</code></p><p>named: <code>aqua</code></p></td></tr><tr><td>i_Font_Family</td><td>Accepts <code>varchar2</code> and supports font families supported in Excel and CSS.</td><td><code>sans-serif</code></td></tr><tr><td>i_Font_Bold</td><td><code>boolean</code> data type, makes text bold.</td><td><code>true</code></td></tr><tr><td>i_Font_Italic</td><td><code>boolean</code> data type, makes text italic.</td><td><code>true</code></td></tr><tr><td>i_Font_Underline</td><td><code>boolean</code> data type, makes text underlined.</td><td><code>true</code></td></tr></tbody></table>

#### Borders

<table><thead><tr><th width="200">Parameter</th><th width="346">Description</th><th>Sample input</th></tr></thead><tbody><tr><td>i_Border</td><td>Accepts <code>Option_Varchar2</code> and applies a border to all four sides of the cell.</td><td><code>b_Report.b_Double</code></td></tr><tr><td><p>i_Border_Top</p><p>i_Border_Bottom</p><p>i_Border_Left</p><p>i_Border_Right</p></td><td>Accepts <code>Option_Varchar2</code> and applies a border to specified side of the cell.</td><td><code>b_Report.b_Dashed</code></td></tr><tr><td>i_Border_Color</td><td>Accepts <code>varchar2</code> and supports hex color codes and named colors.</td><td><p>hex: <code>#333BBB</code></p><p>named: <code>magenta</code></p></td></tr><tr><td><p>i_Border_Top_Color</p><p>i_Border_Bottom_Color</p><p>i_Border_Left_Color</p><p>i_Border_Right_Color</p></td><td>Accepts <code>varchar2</code> and supports hex color codes and named colors.</td><td><p>hex: <code>#ABCDEF</code></p><p>named: <code>yellow</code></p></td></tr></tbody></table>

#### Text alignments

<table><thead><tr><th width="200">Parameter</th><th width="352">Description</th><th>Sample input</th></tr></thead><tbody><tr><td>i_Horizontal_Alignment</td><td>Takes <code>Option_Number</code> and defines the horizontal alignment of the text.</td><td><code>b_Report.a_Right</code></td></tr><tr><td>i_Vertical_Alignment</td><td>Takes <code>Option_Number</code> and defines the vertical alignment of the text.</td><td><code>b_Report.a_Middle</code></td></tr></tbody></table>

#### Other formatting options

<table><thead><tr><th width="185">Parameter</th><th width="409">Description</th><th>Sample input</th></tr></thead><tbody><tr><td>i_Text_Rotate</td><td>Takes a <code>number</code> and supports a range of -90 to 90 for Excel and -180 to 180 for HTML.</td><td><code>-10</code></td></tr><tr><td>i_Text_Wrap</td><td><code>boolean</code> data type that, when enabled, wraps text to a new line if it exceeds the cell's width.</td><td><code>true</code></td></tr><tr><td>i_Shrink_To_Fit</td><td><code>boolean</code> data type that, when enabled, reduces text size to fit within the cell dimensions. Works only in Excel.</td><td><code>true</code></td></tr><tr><td>i_Indent</td><td>Takes a <code>number</code> and adds leading space to shift the text to the right within the cell.</td><td><code>3</code></td></tr><tr><td>i_Cell_Format</td><td>Accepts <code>varchar2</code> and follow Excel's formatting syntax. It can be interprated as number, percentage, currency and more. Works only in Excel.</td><td><code>$#,#0.00</code></td></tr><tr><td>i_Background_Color</td><td>Accepts <code>varchar2</code> and supports hex color codes and named colors.</td><td><p>hex: <code>#FEDCBA</code></p><p>named: <code>green</code></p></td></tr></tbody></table>

### Creating styles

Once the workbook is opened, you can create custom styles using the `b_report` package. Here’s how you can create a style:

```plsql
b_Report.New_Style(i_Style_Name => 'test_style', /* ... */);
```

Here, `i_Style_Name` is a unique name that will be stored globally until the workbook is closed. Instead of `...`, you can specify cell style options. These options are optional that can be applied selectively. Below is an example of a new style with a font size of 25 pixels, a blue border, and a yellow background:

```plsql
b_Report.New_Style(i_Style_Name       => 'test_style',
                   i_Font_Size        => 25,
                   i_Border           => b_Report.b_Thin,
                   i_Border_Color     => '#0000FF',
                   i_Background_Color => '#FFFF00');
```

### Usage

Styles are used with `b_table` type instances. Here is how to use it:

```plsql
-- First we need b_Table instance
a b_Table := b_Report.New_Table();

-- Using a style
a.Current_Style('test_style');
```

Styles will be applied to all newly created cells and will remain in effect until a different style is set for the table.

### Built-in styles

Biruni provides built-in styles that enables a quick and consistent way to format cells without manually defining styles each time. They help maintain a uniform appearance across reports, ensuring readability and professionalism. Using predefined styles also reduces errors and saves time by applying standardized formatting instantly. Additionally, they allow for easier modifications, as updating a single built-in style can reflect changes across multiple cells.

#### Opening book with styles

If the workbook is opened with additional styles, it will come with following styles:

* root — `root`
* sections — `header`, `footer`, `body`
* fonts — `bold`, `italic`, `udnerline`
* text alignments — `left`, `center`, `right`, `top`, `middle`, `bottom`
* no style — `text`
* number formats — `number`, `number1`, `number2`, `number3`, `number4`, `number5`, `number6`
* rotated — `down2up`, `up2down`

#### Built-in standart styles

If the workbook is opened without additional styles, you can apply pre-built styles and use them as needed:

```
b_Report.Add_Standart_Styles();
```

These styles can be referenced by their style names and include the following options:

* root — `root`
* info — `info`, `info_underline`
* header — `header`, `header_left`, `header_right`
* body — `body`, `body_right`, `body_cneter`, `body_summ`
* footer — `footer`, `footer_left`, `footer_right`, `footer_summ`

