# 📧 Mail Server & Email - Exercises

This file contains practical exercises for installing Postfix, sending and receiving email, and troubleshooting the local mail server.

## 📝 Exercise 1: Install Postfix and Mail Utilities

Task: Install Postfix and the mail utilities package on your system.

Solution:
```bash
sudo apt update
sudo apt install postfix mailutils -y
```

Expected Output:
```text
Setting up postfix (3.7.10-0+deb12u1) ...
Setting up mailutils (1:3.15-4) ...
```

## 📝 Exercise 2: Configure Postfix for Local Mail

Task: Configure Postfix so that local users can send and receive mail on the system.

Solution:
```bash
sudo postconf -e 'local_recipient_maps ='
sudo postconf -e 'mydestination = localhost, localhost.localdomain, kali'
sudo systemctl restart postfix
sudo systemctl enable postfix
postconf -n | grep -E 'local_recipient_maps|mydestination'
```

Expected Output:
```text
local_recipient_maps =
mydestination = localhost, localhost.localdomain, kali
```

## 📝 Exercise 3: Create a New User for Email Testing

Task: Create a new user and set a password for it to test sending and receiving email.

Solution:
```bash
sudo useradd -m testuser
sudo passwd testuser
id testuser
```

Expected Output:
```text
uid=1001(testuser) gid=1001(testuser) groups=1001(testuser)
```

## 📝 Exercise 4: Send an Email to a User

Task: Send an email with a subject to the user you created in the previous exercise.

Solution:
```bash
echo "Hello testuser, this is a test email!" | mail -s "Test Subject" testuser
mailq
```

Expected Output:
```text
Mail queue is empty
```

## 📝 Exercise 5: Read the Received Email

Task: Log in as the recipient user and read the received email.

Solution:
```bash
sudo su - testuser
mail
```

Alternative (read directly):
```bash
sudo cat /var/mail/testuser
```

Expected Output:
```text
From kali@kali  Mon Nov 04 10:30:00 2026
Subject: Test Subject
To: testuser@kali
Hello testuser, this is a test email!
```

## 📝 Exercise 6: View Mail Logs

Task: View the mail logs to make sure the email was delivered correctly.

Solution:
```bash
sudo journalctl -u postfix -n 20
sudo tail -f /var/log/mail.log
```

Expected Output:
```text
Nov  4 10:30:00 kali postfix/smtp[1234]: 5F8A1: to=<testuser@kali>, status=sent
```

## 📝 Exercise 7: Troubleshoot Empty Mailbox

Task: If /var/mail/testuser is empty (0 bytes), find the cause and fix it.

Solution:
```bash
ls -l /var/mail/testuser
sudo postconf -e 'local_recipient_maps ='
sudo systemctl restart postfix
echo "test again" | mail -s "Retry" testuser
sudo cat /var/mail/testuser
```

Expected Output:
```text
-rw-rw---- 1 testuser mail 512 Nov  4 10:31 /var/mail/testuser
test again
```

## 📝 Exercise 8: Fix Mailbox Permission Denied

Task: If you get "Permission denied" on the mailbox, fix the ownership and permissions.

Solution:
```bash
sudo chown testuser:mail /var/mail/testuser
sudo chmod 660 /var/mail/testuser
ls -l /var/mail/testuser
```

Expected Output:
```text
-rw-rw---- 1 testuser mail 512 Nov  4 10:31 /var/mail/testuser
```

## 📝 Exercise 9: Check the Mail Queue and Flush It

Task: Show the mail queue and flush it if there are any pending messages.

Solution:
```bash
mailq
sudo postqueue -f
mailq
```

Expected Output:
```text
Mail queue is empty
```

## 📝 Exercise 10: Send a File Content as an Email

Task: Send the content of a file as an email to a user.

Solution:
```bash
echo "This is the file content." > body.txt
mail -s "File Content" testuser < body.txt
sudo cat /var/mail/testuser
```

Expected Output:
```text
Subject: File Content
To: testuser@kali
This is the file content.
```
