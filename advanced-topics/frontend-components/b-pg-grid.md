---
icon: grid
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

# b-pg-grid

This new feature provides a new search experience in B-PG-GRID.\
When you search for particular text, the grid shows you rows which:

1. are matched by searching text (existing feature).
2. has Null values of the searching column (✨new feature).

To enable showing the rows which have Null search columns, you have to postfix your search or searchable attribute column names with a question mark (?). Which is shown in the picture below.\\

<figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/searchable.png" alt=""><figcaption></figcaption></figure>

It is useful when you have to search for something, then add a new row and fill the row while the grid is filtered. Before you had to clear the search input and then you could see a newly added row in your grid.

***

## Sorting by date feature

When `<b-pg-grid>` needs to be sorted by date (not by string), now we can use attributes `format="date"` & `date-format=""` in `<b-pg-col>` to achieve it.`date-format=""` is optional here, and it gets `"MM.DD.YYYY HH:mm:ss"` as the default value if not specified.\\

<figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/sort-by-date.png" alt=""><figcaption></figcaption></figure>

P.S. We already could sort by number like so: `format="number"` or `format="amount"` they do the same job.

***

## Navigating across b-pg-cols using keyboard keys

When you want to navigate and do some action on `<b-pg-col>` and the elements in it, you should:

1. include an attribute `b-navigate` in `<b-pg-grid>`
2. and include an attribute `on-navigate` and attach a function which you want to be called when the cell is navigated\\

<figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/navigate-function.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/b-navigate.png" alt=""><figcaption></figcaption></figure>

In the pictures above, there is an example of default `<b-pg-grid>` navigation in which you only can navigate between cells vertically(↕) and it is done by pressing `ArrowUp(↑)` and `ArrowDown(↓)` keys.\
A next element which is in navigating direction and has `on-navigate` attribute is passed to the function as parameter and when we press arrow keys the function will be executed.

If you want to do the same thing horizontally(↔), you must add additional attributes which are `navi-right` and `navi-left`.\
There is no default keys set to navigate horizontally, that is why you must declare what key or combination of keys pressing is needed.

In an example below, we have used `ArrowRight(→)` and `ArrowLeft(←)` keys to achieve a result that we want:

<figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/navigate-function-2.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/arrow-left-right.png" alt=""><figcaption></figcaption></figure>

As mentioned above, you may press more than one keys to go one direction. But remember, you only can use combination of two keys and one of them must be modifier key which is 'CtrlKey', 'AltKey', 'ShiftKey' or 'MetaKey'.

`navi-left="Ctrl+ArrowLeft"`

`navi-up="Ctrl+ArrowUp"`

`navi-right="Ctrl+ArrowRight"`

`navi-down="Ctrl+ArrowDown"`
