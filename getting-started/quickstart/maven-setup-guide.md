# Maven Setup Guide

## Prerequisites

### Required Tools

* Oracle (v19)
* Java Development Kit (JDK v21)
* Maven (v3 or above)
* NodeJS (v20, with npm v11)

***

## Project Setup

### Clone the Repository

{% tabs %}
{% tab title="SSH" %}
```bash
git clone git@github.com:greenwhite/biruni.git
```
{% endtab %}

{% tab title="HTTPS" %}
```bash
git clone https://github.com/greenwhite/biruni.git
```
{% endtab %}
{% endtabs %}

***

### Oracle Setup Guide

#### Requirements:

* Oracle Database installed and running
* SYS user access

#### Database installation steps:

{% stepper %}
{% step %}
### Initial Oracle Setup (as SYS)

1. Log in to the database as SYS

#### Log in options:

{% tabs %}
{% tab title="SQL * Plus" %}
#### Log in to the database as SYS:

```sql
sqlplus sys/your_password@hostname:port/service_name as sysdba
```

For example:

```sql
sqlplus sys/password@192.168.1.100:1521/orcl as sysdba
```

***

#### Log in to the database

```sql
sqlplus your_username/your_password@hostname:port/service_name
```

For example:

```sql
sqlplus user8080/password123@192.168.1.100:1521/orcl
```
{% endtab %}

{% tab title="IDE" %}
In addition to SQL\*Plus, you can also work with Oracle databases using various Integrated Development Environments (IDEs) such as Oracle SQL Developer, Visual Studio, PL/SQL Developer, and more.
{% endtab %}
{% endtabs %}

2. Navigate to <mark style="color:orange;">**`biruni/scripts/system/`**</mark> folder, then:
   1. Execute <mark style="color:orange;">**`tablespace.sql`**</mark> to create necessary tablespaces (Skip if already created)
   2. Execute <mark style="color:orange;">**`user.sql`**</mark> to create a new database schema. You will be prompted for schema username and password

{% hint style="info" %}
<mark style="color:orange;">**`tablespace.sql`**</mark> creats tablespaces used in biruni-based projects
{% endhint %}

#### Executing SQL scripts guide:

{% tabs %}
{% tab title="SQL * Plus" %}
Once you've logged into your Oracle database through SQL\*Plus, you have two ways to execute a SQL script:

1. Using just filename (if your script is in your current directory):

```sql
SQL> @filename.sql
```

2. Using complete file path:

```sql
SQL> @path_to_your_script.sql
```

For example:

```sql
SQL> @D:\projects\biruni\scripts\system\tablespace.sql
```
{% endtab %}

{% tab title="IDE" %}
In addition to SQL\*Plus, you can also work with Oracle databases using various Integrated Development Environments (IDEs) such as Oracle SQL Developer, Visual Studio, PL/SQL Developer, and more.
{% endtab %}
{% endtabs %}

3. Install Fazo Schema (Skip if already installed)
   1. Navigate to <mark style="color:orange;">**`biruni/main/fazo/`**</mark>&#x20;
   2. Run <mark style="color:orange;">**`build.bat`**</mark> that generates <mark style="color:orange;">**`install_fazo_schema.sql`**</mark>  in <mark style="color:orange;">**`biruni/main/`**</mark> folder
4. [Execute](./#executing-sql-scripts-guide) the generated SQL script to create Fazo\_schema - the foundational component of all Biruni-based projects

{% hint style="info" %}
Fazo\_schema provides essential utilities, custom data types, and core functionality for the Biruni framework.
{% endhint %}
{% endstep %}

{% step %}
### Database Setup (as Newly Created User)

1. [Log in](./#log-in-options) with your created user account
2.  Download <mark style="color:orange;">**`orcl_biruni_all.sql`**</mark> from the Assets section at [Biruni Releases](https://github.com/greenwhite/biruni/releases).

    [Execute](./#executing-sql-scripts-guide) the downloaded SQL file

{% hint style="info" %}
<mark style="color:orange;">**`orcl_biruni_all.sql`**</mark> sets up the database of biruni project that includes tables, packages and all necessary data structures. In the process there will be created a new "Head" company and a new user "admin".

#### Admin login & password:

* Login: admin@head
* Password: greenwhite
{% endhint %}

1. Install development environment (Optional)
   1. Navigate to <mark style="color:orange;">**`biruni/main/oracle/dev/`**</mark>&#x20;
   2. [Execute](./#executing-sql-scripts-guide) <mark style="color:orange;">**`make_dev.sql`**</mark>
{% endstep %}
{% endstepper %}

<pre class="language-markdown" data-title="Project Structure"><code class="lang-markdown"><strong>📁 biruni/
</strong>├── 📁 main/
│   ├── 📁 fazo/
│   │   ├── 📄 build.bat
│   │   └── ...
│   ├── 📁 oracle/
│   │   ├── 📁 dev/
│   │   │   ├── 📄 make_dev.sql
│   │   │   └── ...
│   │   └── ...
│   ├── 📄 install_fazo_schema.sql
│   └── ...
├── 📁 scripts/
│   ├── 📁 system/
│   │   ├── 📄 tablespace.sql
│   │   ├── 📄 user.sql
│   │   └── ...
│   └── ...
└── ...
</code></pre>

***

### Frontend CSS Styles Configuration&#x20;

#### Install sass globally:

```bash
npm install -g sass
```

Navigate to <mark style="color:orange;">**`biruni/main/web/biruni/`**</mark>

Generate <mark style="color:orange;">**`main.css`**</mark> using one of these following methods:

{% tabs %}
{% tab title="Command Prompt" %}
```
sass main.scss:main.css --no-source-map -s compressed
```
{% endtab %}

{% tab title="Windows File Explorer" %}
Double-click <mark style="color:orange;">**`sass-watcher.cmd`**</mark> in this folder
{% endtab %}
{% endtabs %}

***

### Application Configuration

Navigate to <mark style="color:orange;">**`biruni/main/app/src/main/resources/`**</mark>

Copy <mark style="color:orange;">**`application.properties.template`**</mark> to <mark style="color:orange;">**`application.properties`**</mark>

Configure the following properties:

{% code title="appliction.properties" %}
```properties
spring.datasource.username=YOUR_SCHEMA_USERNAME
spring.datasource.password=YOUR_SCHEMA_PASSWORD

spring.profiles.active=dev

biruni.dev.projects-folder=YOUR_PROJECTS_LOCATION

spring.web.resources.static-locations=file:../web/
```
{% endcode %}

***

## Running the Project

Navigate to <mark style="color:orange;">**`biruni/main/app/`**</mark>&#x20;

### Install Dependencies

```
mvn clean install
```

### Start the Application

```
mvn spring-boot:run
```

After successful initialization, you should see the following console output:

<figure><picture><source srcset="../../.gitbook/assets/getting-started/quickstart/console-dark.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/getting-started/quickstart/console-white.png" alt=""></picture><figcaption></figcaption></figure>

Open your browser and in the address bar at the top enter **`http://localhost:8080`**.

<figure><img src="../../.gitbook/assets/getting-started/quickstart/login.png" alt=""><figcaption></figcaption></figure>

#### [Log in as admin](./#admin-login-and-password) using:

Welcome to the Dashboard!

<figure><img src="../../.gitbook/assets/getting-started/quickstart/dashboard.png" alt=""><figcaption></figcaption></figure>

***

## Troubleshooting

### Database Setup Issues

#### Connection errors

```
ORA-12541: TNS:no listener
ORA-01017: invalid username/password
```

Fix:

1. Check Oracle services are running
2. Verify credentials in <mark style="color:orange;">**`application.properties`**</mark>&#x20;

```properties
spring.datasource.username=YOUR_SCHEMA_USERNAME
spring.datasource.password=YOUR_SCHEMA_PASSWORD
```

#### Permission Issues

```
ORA-01031: insufficient privileges
```

Fix: Ensure you have executed <mark style="color:orange;">**`user.sql`**</mark> as SYS

### Application Startup

#### Web files not found

<div align="left"><figure><img src="../../.gitbook/assets/getting-started/quickstart/file-not-found.png" alt=""><figcaption></figcaption></figure></div>

Make sure you have configured web resources static location in <mark style="color:orange;">**`application.properties`**</mark>&#x20;

```properties
spring.web.resources.static-locations=file:../web/=
```

#### Uploaded file changed

<div align="left"><figure><img src="../../.gitbook/assets/getting-started/quickstart/upload-files-changed.png" alt=""><figcaption></figcaption></figure></div>

Ensure you have developer project folder configured properly in <mark style="color:orange;">**`application.properties`**</mark>

```properties
biruni.dev.projects-folder=YOUR_PROJECTS_LOCATION
```
