# Module setup

The current LMS project contains only one module, `cr`. As mentioned earlier in the guide, a project can include multiple Oracle modules. Create the folder structure accordingly, as shown [here](architecture.md#oracle).

Each module includes setup files (tables, sequences, partitions) along with essential packages such as api, watchers, utils, and others.

## Module setup files

[Module setup files](../data-business-logic/module-files/module-setup-folder/) are SQL scripts that run only once during the initial project setup. In Biruni, these scripts are executed through the `start_all.sql` file.

Setup Files Overview:

* `table.sql` – defines all tables used in the module
* `sequence.sql` – all the sequences associated with the module's tables
* `settings.sql` – inserts initial data during module setup
* `partitioning.sql` – contains logic for table partitioning if applicable

In our project we only have `cr_table.sql` and `cr_sequence.sql`.

## Module packages

For our guide [module packages](../data-business-logic/module-files/module-packages.md), we'll need at least the `api` and `next` prefixed packages. In larger projects, additional packages such as `core`, `global`, `watcher`, and others may also be included depending on the system's complexity.

#### Next package

We will begin with the `cr_next.pck` package, which contains functions that return the next value of sequences. Below is an example for the `cr_subjects_sq` sequence:

{% code title="cr_next.sql" %}
```sql
create or replace package Cr_Next is
  ----------------------------------------------------------------------------------------------------
  Function Subject_Id return number;
end Cr_Next;
/
create or replace package body Cr_Next is
  ----------------------------------------------------------------------------------------------------
  Function Subject_Id return number is
  begin
    return Cr_Subjects_Sq.Nextval;
  end;

end Cr_Next;
/
```
{% endcode %}

#### Pref package

We use the pref package to store our week day constants, ensuring consistent usage of values like odd (`O`), even (`E`), and all (`A`) across the system.

{% code title="cr_pref.pck" %}
```sql
create or replace package Cr_Pref is
  ----------------------------------------------------------------------------------------------------
  -- week days
  c_Wd_Odd  constant varchar2(1) := 'O';
  c_Wd_Even constant varchar2(1) := 'E';
  c_Wd_All  constant varchar2(1) := 'A';
  ----------------------------------------------------------------------------------------------------
  Function Wd_Odd return varchar2;
  Function Wd_Even return varchar2;
  Function Wd_All return varchar2;
end Cr_Pref;
/
create or replace package body Cr_Pref is
  ----------------------------------------------------------------------------------------------------
  Function Wd_Odd return varchar2 is
  begin
    return c_Wd_Odd;
  end;

  ----------------------------------------------------------------------------------------------------
  Function Wd_Even return varchar2 is
  begin
    return c_Wd_Even;
  end;

  ----------------------------------------------------------------------------------------------------
  Function Wd_All return varchar2 is
  begin
    return c_Wd_All;
  end;

end Cr_Pref;
/
```
{% endcode %}

#### Api package

Next, we'll create the api package for our module tables, which implements DML operations such as insert, update, and delete. These procedures are used by UI packages to manage data interactions.

For our **reference tables** we are going to use very simple api procedure, folowing code represents `cr_subjects` table example save, delete procedures:

<pre class="language-sql"><code class="lang-sql">----------------------------------------------------------------------------------------------------
Procedure Subject_Save(i_Subject Cr_Subjects%rowtype) is
begin
  z_Cr_Subjects.Save_Row(i_Subject);
end;

<strong>----------------------------------------------------------------------------------------------------
</strong>Procedure Subject_Delete
(
  i_Company_Id number,
  i_Subject_Id number
) is
begin
  z_Cr_Subjects.Delete_One(i_Company_Id => i_Company_Id, i_Subject_Id => i_Subject_Id);
end;
</code></pre>

For **entity tables** like `cr_teachers` and `cr_students`, the logic becomes more complex due to their connection with Biruni’s `md_users` table, along with associated roles and filials. The following code demonstrates the `save` and `delete` functions for the `cr_students` table:

```sql
----------------------------------------------------------------------------------------------------
Procedure Student_Save
(
  i_Company_Id  number,
  i_Filial_Id   number,
  i_Student_Id  number,
  i_First_Name  varchar2,
  i_Last_Name   varchar2,
  i_Middle_Name varchar2,
  i_Role_Id     number,
  i_Birth_Date  date,
  i_Gender      varchar2,
  i_State       varchar2,
  i_Email       varchar2,
  i_Phone       varchar2,
  i_Photo_Sha   varchar2
) is
  r_User Md_Users%rowtype;
  v_Name varchar2(250 char) := trim(i_Last_Name || ' ' || i_First_Name || ' ' || i_Middle_Name);
begin
  if i_First_Name is null then
    b.Raise_Error(t('First name is required!'));
  end if;

  if i_State is null then
    b.Raise_Error(t('State cannot be null!'));
  end if;

  Md_Api.Person_Save(i_Company_Id => i_Company_Id,
                     i_Person_Id  => i_Student_Id,
                     i_Name       => v_Name,
                     i_State      => i_State,
                     i_Email      => i_Email,
                     i_Phone      => i_Phone,
                     i_Photo_Sha  => i_Photo_Sha,
                     Is_Legal     => false);

  if z_Md_Users.Exist_Lock(i_Company_Id => i_Company_Id,
                           i_User_Id    => i_Student_Id,
                           o_Row        => r_User) then
    r_User.Name  := v_Name;
    r_User.State := i_State;
  else
    r_User.Company_Id               := i_Company_Id;
    r_User.User_Id                  := i_Student_Id;
    r_User.Name                     := v_Name;
    r_User.Login                    := Lower(i_First_Name) || '.' || Lower(i_Last_Name);
    r_User.Password                 := Fazo.Hash_Sha1(Lower(i_First_Name) || '.' ||
                                                      Lower(i_First_Name));
    r_User.Gender                   := i_Gender;
    r_User.State                    := i_State;
    r_User.User_Kind                := Md_Pref.c_Uk_Normal;
    r_User.Password_Changed_On      := sysdate;
    r_User.Password_Change_Required := 'N';
    r_User.Two_Factor_Verification  := Md_Pref.c_Two_Step_Verification_Disabled;
  end if;

  Md_Api.User_Save(r_User);

  Md_Api.User_Add_Filial(i_Company_Id => i_Company_Id,
                          i_User_Id    => i_Student_Id,
                          i_Filial_Id  => i_Filial_Id);
 
  for r in (select t.Role_Id
               from Md_User_Roles t
             where t.Company_Id = i_Company_Id
               and t.User_Id = r_User.User_Id
               and t.Filial_Id = i_Filial_Id)
  loop
    Md_Api.Role_Revoke(i_Company_Id => i_Company_Id,
                        i_Filial_Id  => i_Filial_Id,
                       i_User_Id    => r_User.User_Id,
                       i_Role_Id    => r.Role_Id);
  end loop;

  Md_Api.Role_Grant(i_Company_Id => i_Company_Id,
                    i_User_Id    => i_Student_Id,
                    i_Filial_Id  => i_Filial_Id,
                    i_Role_Id    => i_Role_Id);

  z_Cr_Students.Save_One(i_Company_Id  => i_Company_Id,
                         i_Student_Id  => i_Student_Id,
                         i_First_Name  => i_First_Name,
                         i_Last_Name   => i_Last_Name,
                         i_Middle_Name => i_Middle_Name,
                         i_Birth_Date  => i_Birth_Date);
end;

----------------------------------------------------------------------------------------------------
Procedure Student_Delete
(
  i_Company_Id number,
  i_Student_Id number
) is
begin
  z_Cr_Students.Delete_One(i_Company_Id => i_Company_Id, i_Student_Id => i_Student_Id);
  z_Md_Users.Delete_One(i_Company_Id => i_Company_Id, i_User_Id => i_Student_Id);
  z_Md_Persons.Delete_One(i_Company_Id => i_Company_Id, i_Person_Id => i_Student_Id);
end;
```

Here, `md_users` and `md_persons` tables provides additional user data for student (e.g. `gender`, `state`, `email`, `photo_sha`).

When saving a student to the database, the data must be inserted into three tables in sequence: first into `md_persons`, then `md_users`, and finally into `cr_students`. After adding the user to `md_users`, the student is assigned to the appropriate filial and attached to a role. Detaching the role beforehand is essential when editing an existing student to ensure any previous role assignments are cleared.

For relational tables like `cr_teacher_subjects`, the logic is straightforward: these tables simply link existing records (e.g., teachers and subjects) without additional business logic, typically involving basic insert and delete operations.

```sql
---------------------------------------------------------------------------------------------------
Procedure Attach_Subject
(
  i_Company_Id number,
  i_Subject_Id number,
  i_Teacher_Id number
) is
begin
  z_Cr_Teacher_Subjects.Insert_One(i_Company_Id => i_Company_Id,
                                   i_Teacher_Id => i_Teacher_Id,
                                   i_Subject_Id => i_Subject_Id);
end;

----------------------------------------------------------------------------------------------------
Procedure Detach_Subject
(
  i_Company_Id number,
  i_Subject_Id number,
  i_Teacher_Id number
) is
begin
  z_Cr_Teacher_Subjects.Delete_One(i_Company_Id => i_Company_Id,
                                   i_Teacher_Id => i_Teacher_Id,
                                   i_Subject_Id => i_Subject_Id);
end;
```

The `cr_courses` save procedure is also straightforward, with one additional step: after saving the course, it automatically generates lesson records for the next 30 days based on the course schedule.

```sql
----------------------------------------------------------------------------------------------------
Procedure Course_Save
(
  i_Company_Id number,
  i_Course_Id  number,
  i_Name       varchar2,
  i_Teacher_Id number,
  i_Subject_Id number,
  i_Room_Id    number,
  i_Week_Day   varchar2,
  i_Begin_Time date,
  i_End_Time   date,
  i_State      varchar2,
  i_Note       varchar2
) is
  v_Week_Day varchar2(1);
begin
  v_Week_Day := z_Cr_Courses.Take(i_Company_Id => i_Company_Id, i_Course_Id => i_Course_Id).Week_Day;

  z_Cr_Courses.Save_One(i_Company_Id => i_Company_Id,
                        i_Course_Id  => i_Course_Id,
                        i_Name       => i_Name,
                        i_Teacher_Id => i_Teacher_Id,
                        i_Subject_Id => i_Subject_Id,
                        i_Room_Id    => i_Room_Id,
                        i_Week_Day   => i_Week_Day,
                        i_Begin_Time => i_Begin_Time,
                        i_End_Time   => i_End_Time,
                        i_State      => i_State,
                        i_Note       => i_Note);

  if v_Week_Day is null or v_Week_Day <> i_Week_Day then
    for r in (select t.Company_Id, t.Lesson_Id, t.Course_Id, t.Lesson_Date
                from Cr_Lessons t
                where t.Company_Id = i_Company_Id
                  and t.Course_Id = i_Course_Id
                  and t.Lesson_Date > sysdate)
    loop
      z_Cr_Lessons.Delete_One(i_Company_Id => r.Company_Id, i_Lesson_Id => r.Lesson_Id);
    end loop;
  
    Gen_Lessons(i_Company_Id => i_Company_Id,
                i_Course_Id  => i_Course_Id,
                i_Week_Day   => i_Week_Day);
  end if;

end;

----------------------------------------------------------------------------------------------------
Procedure Course_Delete
(
  i_Company_Id number,
  i_Course_Id  number
) is
begin
  z_Cr_Courses.Delete_One(i_Company_Id => i_Company_Id, i_Course_Id => i_Course_Id);
end;
```

