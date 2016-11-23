# CS2107 Lecture 6

## Salzer and Schroeder's Design Principles
- **Economy of mechanism**: Keep design as simple and small as possible.
- **Fail-safe defaults**: Base access decisions on permission rather than exclusion. The default is no access.
- **Complete mediation**: Every access to every object must be checked for authority.
- **Open design**: The design should not be secret.
- **Separation of privilege**: Two keys are better than one. No single event can compromise the system. Dual controls.
- **Least privilege**: Every program and every user of the system should operate using the least set of privileges necessary to complete the job.
- **Least common mechanism**: Minimize the amount of mechanism common to more than one user and depended on by all users.
  - A common mechanism may provide a path of information leaks (confinement/covert storage channels).
- **Psychological acceptability**: Human interfaces should be easy to use.

## SQL Injection
The most common vulnerability on web sites today (according to [OWASP Top 10 in 2013](https://www.owasp.org/index.php/Top_10_2013-Top_10)).

### Example

The code which accepts user input:

```sql
SELECT * FROM users WHERE name = '" + userName + "';
```

If `userName` is set to:

```
a';UPDATE `test`.`mrks` SET `marks` = '100' WHERE `mrks`.`name` = 'hugh
```

Then we get:

```sql
SELECT * FROM users WHERE name = 'a';
UPDATE `test`.`mrks` SET `marks` = '100'
WHERE `mrks`.`name` = 'hugh';
```

## Cross-Site Scripting (XSS)
XSS enables attackers to inject client-side scripts into web pages viewed by other users. The main intention is to either phish for data from other users, redirect users to a different webpage unintentionally, or to just interrupt/denial of services.

- **Stored/persistent XSS**: Requires a database or some sort of persistent storage to work. Malicious client-side content may then be displayed to other users when it is retrieved from the database.
- **Reflected XSS**: Does not require a form of persistent storage. Can be displayed from HTML being encoded in the URL query string, or otherwise. Maliciour client-side content can then be displayed to other users when the webpage renders the unsanitized HTML found in the query string.

## Mitigations
- Control over inputs:
  - In PHP: `mysql_real_escape_string()`, `filter_var()`, etc.
  - Using PHP/SQL `md5` functions to hash content to check for equality
    - e.g. `$sql = "SELECT * FROM prods WHERE MD5(id)=" . md5($pid);` 
  - Using prepared statements (e.g. PDO)
- Control over outputs:
  - [HTMLPurifier](http://htmlpurifier.org/)
  - UI libraries like [React](https://facebook.github.io/react/) :smile: