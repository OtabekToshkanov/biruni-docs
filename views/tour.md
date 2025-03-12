---
icon: newspaper
---

# Tour

Tour is a developer tool that helps to create navigation guide for the form. It is accessible in the **Developer toolbar**:

<figure><img src="../.gitbook/assets/views/tour-create-tour.png" alt=""><figcaption></figcaption></figure>

## Create the tour

The tour may include one or more steps, depending on the level of explanation required for the form. Each step consists of an element, title, description, and the position of the popup modal.

<figure><img src="../.gitbook/assets/views/tour-page.png" alt=""><figcaption></figcaption></figure>

Once you create a tour new button appreas in the **Developer toolbar**:

Click the "**Show Tour**" button to navigate through each step of the tour.

<figure><img src="../.gitbook/assets/views/tour-start-button.png" alt=""><figcaption></figcaption></figure>

## Element selection

You can use CSS selector for this field. Html DOM elements can be selected by classname, id or tagname. If there are many elements selected then first one will be chosen.

Sample examples for the selector:

* Select by id: `#myElement`
* Select by class: `.myClass`&#x20;
* Select by tagname: `button`&#x20;

{% hint style="info" %}
Selecting elements by their **id** is recommended as they provide uniqueness and give more clear path
{% endhint %}

## Usage

Selection by id example:

{% code title="user_list.html" overflow="wrap" %}
```html
<button id="addUser" type="button" class="btn btn-success">Add</button>
```
{% endcode %}

This video provides a quick overview of the tour mechanism:

{% file src="../.gitbook/assets/views/route-guide.webm" %}
