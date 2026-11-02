# 📧 Mail Server & Email Management in Linux

This section covers how to set up a local mail server using **Postfix** and send/receive emails using the `mail` command.

## 📦 1. Installing Postfix and Mail Utilities

```bash
sudo apt update
sudo apt install postfix mailutils -y
```

**Note:** During installation, select **"Local only"** (or "Internet Site") and set the system mail name (e.g., `kali`).

## 🛠️ 2. Basic Postfix Configuration

```bash
sudo postconf -e 'local_recipient_maps ='
sudo postconf -e 'mydestination = localhost, localhost.localdomain, kali'
sudo systemctl restart postfix
sudo systemctl enable postfix
```

## 👤 3. Creating a User and Sending Email

**Step 1: Create a user:**

```bash
sudo useradd -m testuser
sudo passwd testuser
```

**Step 2: Send an email:**

```bash
echo "Hello testuser, this is a test email!" | mail -s "Test Subject" testuser
```

**Step 3: Check the email (as testuser):**

```bash
sudo su - testuser
mail
```

**Alternative (read directly):**

```bash
sudo cat /var/mail/testuser
```

## 📜 4. Viewing Mail Logs

```bash
sudo journalctl -u postfix -n 20
sudo tail -f /var/log/mail.log
```

## 🔧 5. Troubleshooting Common Issues

**Issue 1: Email not delivered (empty mailbox)**

If `/var/mail/testuser` is empty (0 bytes), it means Postfix rejected the email. Fix it with:

```bash
sudo postconf -e 'local_recipient_maps ='
sudo systemctl restart postfix
```

**Issue 2: Permission denied on mailbox**

The mailbox file must be owned by the user. Fix it with:

```bash
sudo chown testuser:mail /var/mail/testuser
sudo chmod 660 /var/mail/testuser
```

## 📊 6. Comparison: Mail Commands

| Command | Description |
| :--- | :--- |
| `mail -s "Subject" user` | Send email with subject |
| `mail` | Read received emails |
| `mailq` | Show the mail queue |
| `postqueue -f` | Flush the mail queue |
| `cat /var/mail/user` | Read mailbox directly |
| `journalctl -u postfix` | View Postfix logs |

## 💡 7. Pro Tips

| Shortcut / Command | Description |
| :--- | :--- |
| `mail -s "Subject" user < file.txt` | Send file content as email |
| `echo "body" \| mail -s "Subject" user` | Send a one-line email |
| `sudo cat /var/mail/user` | Read email without logging in |
| `postconf -n` | Show all Postfix configuration |
| `systemctl status postfix` | Check if Postfix is running |

