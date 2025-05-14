---
icon: input-pipe
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

# b-input

### Atributes

*   pinable - To save default value.\
    Value is number which delay to loading. loading execute after (order \* 500) ms. Value is optional and by default is 1.\
    Data saved to localStorage in dev mode, in prod it is saved to database\
    Usage:

    ```
    <b-input ... pinable     ...> ... </b-input>
    <b-input ... pinable='1' ...> ... </b-input>
    <b-input ... pinable='2' ...> ... </b-input>
    ```

    \
    Example:

    ```
    <div class="form-group">
           <label><t>room name</t><r/></label>
           <b-input name="rooms"
                    model="d.room_name"
                    model-key="d.room_id"
                    column="room_id, name, order_no"
                    search="name"
                    sort="order_no, name"
                    on-select="setRoom(row)"
                    on-delete="deleteRoom()"
                    is-view="fi.select_room"
                    on-view="selectRoom(row)"
                    auto-fill
                    pinable
                    input-icon="fa fa-times"
                    required-key>
             {{ row.name }}
           </b-input>
        </div>
    ```

    Result:\\

    <figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/pinable.png" alt=""><figcaption></figcaption></figure>
