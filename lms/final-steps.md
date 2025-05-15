# Final steps

## Module container

Currently, the dev module is enabled in the developer version, which allows access to all forms in the system. However, in production, forms are not accessible unless they are attached to a module.

To make forms visible to users, you need to create a module for your project. Follow these steps:

1. Navigate to Develop → Form → Projects from the menu.
2. Open the Modules form of the LMS project and create a new module for it.
3. Click the `bind_forms` action on the module you created.
4. Attach all LMS project forms to this module.

{% hint style="info" %}
This module container completely dfferent from oracle module
{% endhint %}

## Project menus

To enable quick access to the **LMS project** forms, add them to the **menus**. This allows users to navigate to the forms directly from the application menu.

## Role control

We have created **Teacher** and **Student** roles, but haven't yet used the role mechanism to control permissions.

To manage this:\
Go to **`/biruni/md/role_list`**, where you can control **form access** and **form actions** for each role.

## Genereate files

Until now, we've made changes to **menus**, **modules**, **languages**, and more. Some of this important data (like **menus**) is stored only in database tables.

To preserve and maintain these changes, we need to **generate static files**, which ensures long-term consistency and easier maintenance.

To do this, navigate to Developer → Release → Generate.

## Conclusion

Now that our LMS project guide is complete, we’ve walked through each essential step in developing a new project using the Biruni framework. We learned how to set up the project environment, design and create database tables, and implement APIs to interact with them. We also covered how to configure modules in Oracle, create user interfaces using both HTML and UI packages, and integrate forms into the system. With these foundations in place, you're well-prepared to continue building and scaling your application. This concludes the guide.

Here is the link to the original LMS guide project: [LMS GUIDE](https://github.com/JaloliddinProgramming/lms)
