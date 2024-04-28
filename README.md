# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![Screenshot 2024-04-28 142031](https://github.com/kanagavel7/sqlinjection/assets/162578954/d1f4143c-b53c-40ba-8272-b0ced8e57725)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![Screenshot 2024-04-28 142109](https://github.com/kanagavel7/sqlinjection/assets/162578954/059db57a-6302-4a7b-869d-43b7293c9d43)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![Screenshot 2024-04-28 142126](https://github.com/kanagavel7/sqlinjection/assets/162578954/7a03960a-2ab4-4f60-8124-9f1d297c8abe)

Click on the menu Login/Register and register for an account

![Screenshot 2024-04-28 142140](https://github.com/kanagavel7/sqlinjection/assets/162578954/88efeb96-cadd-4364-bf72-8601c8554422)

Click on the link “Please register here”

![Screenshot 2024-04-28 142238](https://github.com/kanagavel7/sqlinjection/assets/162578954/6b3453ea-c241-4ced-b7f1-3e4616cbf159)

Click on “Create Account” to display the following page:

![Screenshot 2024-04-28 142249](https://github.com/kanagavel7/sqlinjection/assets/162578954/fb8e1e05-40e3-49c5-97ab-b29da21d9451)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

![Screenshot 2024-04-28 142306](https://github.com/kanagavel7/sqlinjection/assets/162578954/6376df10-15dd-4e65-9dc0-95ce3e7c67db)

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![Screenshot 2024-04-28 142329](https://github.com/kanagavel7/sqlinjection/assets/162578954/4a5c19d9-300c-4e9f-b772-012b3b126bc5)

Click “Login”. The logged in page will show as below:

![Screenshot 2024-04-28 142341](https://github.com/kanagavel7/sqlinjection/assets/162578954/a94eb030-bf83-43ef-8e3d-1df23d6c535b)

## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![Screenshot 2024-04-28 142354](https://github.com/kanagavel7/sqlinjection/assets/162578954/169c808b-85db-4175-be7e-36c3e484d2c7)

Click the login button and you will see it enter into the administrator page.

![Screenshot 2024-04-28 142404](https://github.com/kanagavel7/sqlinjection/assets/162578954/e98cbb42-7086-4d7e-bf61-94daa898d05b)

## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![Screenshot 2024-04-28 142415](https://github.com/kanagavel7/sqlinjection/assets/162578954/506fa3ce-aadb-4601-94bd-f093b3c3a2e7)

![Screenshot 2024-04-28 142429](https://github.com/kanagavel7/sqlinjection/assets/162578954/0a6e9179-28af-4d8e-9b2f-a3f6e0cc0e6c)

![Screenshot 2024-04-28 142441](https://github.com/kanagavel7/sqlinjection/assets/162578954/4751914e-d95c-4040-9d61-1674b3e03931)

![Screenshot 2024-04-28 142450](https://github.com/kanagavel7/sqlinjection/assets/162578954/e9287064-0c79-4c2d-963a-67193213f653)

![Screenshot 2024-04-28 142500](https://github.com/kanagavel7/sqlinjection/assets/162578954/91faed2e-8e54-4f40-8e85-676cf3c863fd)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![Screenshot 2024-04-28 142508](https://github.com/kanagavel7/sqlinjection/assets/162578954/10403cb3-4d52-4daa-96eb-e3e7a1f41560)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


![Screenshot 2024-04-28 142518](https://github.com/kanagavel7/sqlinjection/assets/162578954/beccd115-efc8-4a60-a792-4abf53d8fb6c)

After adding the order by 6 into the existing url , the following error statement will be obtained:

![Screenshot 2024-04-28 142530](https://github.com/kanagavel7/sqlinjection/assets/162578954/0843aa8f-16cf-4e2a-8027-4ea456d3d89d)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![Screenshot 2024-04-28 142539](https://github.com/kanagavel7/sqlinjection/assets/162578954/1bf52226-c5fa-4370-9e1e-d10887b76832)

As it is having 5 columns the query worked fine and it provides the correct result

![Screenshot 2024-04-28 142549](https://github.com/kanagavel7/sqlinjection/assets/162578954/48318d2d-508d-4766-8aff-15d62b20a465)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![Screenshot 2024-04-28 142559](https://github.com/kanagavel7/sqlinjection/assets/162578954/d9e11c37-a73f-42e3-8655-8295a6b5b940)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![Screenshot 2024-04-28 142611](https://github.com/kanagavel7/sqlinjection/assets/162578954/6dc2f535-1064-4a3d-be50-5c3ca8da8271)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 142620](https://github.com/kanagavel7/sqlinjection/assets/162578954/e43f039c-f22a-4c53-9b4d-e0908a5ad1a3)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 142630](https://github.com/kanagavel7/sqlinjection/assets/162578954/523d8cf8-fdf9-45b3-a923-640c1554a323)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![Screenshot 2024-04-28 142645](https://github.com/kanagavel7/sqlinjection/assets/162578954/57605dd8-00d1-4eba-8435-21dd60592bd8)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
