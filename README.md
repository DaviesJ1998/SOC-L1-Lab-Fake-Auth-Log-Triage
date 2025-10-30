# SOC L1 Lab — Fake Auth Log Triage (All-in-one)

**Objective:** Demonstrate basic SOC L1 triage skills on Fedora: create mock logs, search for failed logins with `grep`, simulate escalation/documentation, and clean up files. Everything below is a single markdown you can copy into one file.

---

## Overview (short)
Simulated an L1 SOC task on Fedora 42:

- Created a project folder and a mock authentication log (`fake_auth.log`) that mimics successful and failed login entries.  
- Used `grep` to find high-severity events (`ERROR` / `Failed login attempt`).  
- Simulated escalation by writing an `alert_ticket.txt` file and showing its contents.  
- Practiced basic filesystem cleanup and demonstrated correct commands for removing files and directories.

---

## Step 1 — Create project directory and fake logs

Create and change into a new directory, then produce a small mock auth log using `echo`:

```bash
# create project folder and enter it
mkdir ~/soc_project
cd ~/soc_project

# create a fake auth log
echo "2025-10-26 14:00:00 INFO: User login successful"  > fake_auth.log
echo "2025-10-26 14:01:00 ERROR: Failed login attempt" >> fake_auth.log
echo "2025-10-26 14:02:00 ERROR: Failed login attempt" >> fake_auth.log

# verify the file contents
cat fake_auth.log

Expected output:

2025-10-26 14:00:00 INFO: User login successful
2025-10-26 14:01:00 ERROR: Failed login attempt
2025-10-26 14:02:00 ERROR: Failed login attempt

Step 2 — Analyze log for failed logins

Mimic SOC triage by extracting high-severity lines (ERROR / Failed):
# find ERROR lines
grep "ERROR" fake_auth.log

# or search for the word "Failed"
grep "Failed" fake_auth.log

Example output:

2025-10-26 14:01:00 ERROR: Failed login attempt
2025-10-26 14:02:00 ERROR: Failed login attempt

Step 3 — Simulate escalation & documentation

Create a simple ticket file to simulate handing off to an L2 analyst:

# create a simulated alert ticket
echo "Ticket: Failed login spike observed on host X. Triage: L1. Action: escalate to L2 for investigation." > alert_ticket.txt

# show the ticket
cat alert_ticket.txt

Example output:

Ticket: Failed login spike observed on host X. Triage: L1. Action: escalate to L2 for investigation.

Step 4 — Cleanup: remove files and directory

Demonstrate removing files and the project directory. Important: don’t run destructive commands you’re unsure about — verify current directory with pwd.

# remove files
rm fake_auth.log alert_ticket.txt

# check where you are (optional)
pwd

# go up one directory
cd ..

# confirm listing
pwd
ls

# remove the now-empty project directory
rmdir soc_project


Notes:

rm deletes files. Use with caution.

rmdir deletes an empty directory. To remove a non-empty directory recursively, you would use rm -r <dir> (dangerous — avoid unless you intend to delete everything).

Always pwd and ls before running remove commands if you're unsure.

(Screenshot placeholder: assets/images/step4_cleanup.png)

Full example terminal transcript (condensed)
jakedavies@fedora:~/soc_project$ cat fake_auth.log
2025-10-26 14:00:00 INFO: User login successful
2025-10-26 14:01:00 ERROR: Failed login attempt
2025-10-26 14:02:00 ERROR: Failed login attempt

jakedavies@fedora:~/soc_project$ grep "ERROR" fake_auth.log
2025-10-26 14:01:00 ERROR: Failed login attempt
2025-10-26 14:02:00 ERROR: Failed login attempt

jakedavies@fedora:~/soc_project$ grep "Failed" fake_auth.log
2025-10-26 14:01:00 ERROR: Failed login attempt
2025-10-26 14:02:00 ERROR: Failed login attempt

jakedavies@fedora:~/soc_project$ echo "Ticket: Failed login spike observed on host X. Triage: L1. Action: escalate to L2 for investigation." > alert_ticket.txt
jakedavies@fedora:~/soc_project$ cat alert_ticket.txt
Ticket: Failed login spike observed on host X. Triage: L1. Action: escalate to L2 for investigation.

jakedavies@fedora:~/soc_project$ rm fake_auth.log alert_ticket.txt
jakedavies@fedora:~/soc_project$ pwd
/home/jakedavies/soc_project
jakedavies@fedora:~/soc_project$ cd ..
jakedavies@fedora:~$ pwd
/home/jakedavies
jakedavies@fedora:~$ ls
soc_project
jakedavies@fedora:~$ rmdir soc_project

Skills demonstrated

Create and navigate directories (mkdir, cd, pwd).

Create and verify files using echo and cat.

Basic log triage with grep (including simple troubleshooting for case sensitivity).

Document and escalate using text ticket files.

Safe filesystem cleanup (rm, rmdir) and checking context before deletion.

Best practices & notes

Triage tips: For real logs, use awk, sed, or grep -E for richer pattern matching (timestamps, usernames, IPs). Consider using wc -l to count occurrences.

Escalation: Ticket should include exact timestamps, affected host(s), source IPs if known, number of failed attempts, and whether any successful logins followed. Attach sanitized logs if allowed.

Safety: Never run removal commands without confirming the current path. rm -r is destructive — use only when you understand its impact.

OPSEC: When publishing examples, redact hostnames, internal IPs, usernames, and any PII.

Suggested repo layout for this lab
soc_lab_auth_triage/
├─ README.md               # this combined document
├─ labs/
│  └─ auth-triage.md       # optional more detailed write-up
├─ assets/
│  └─ images/              # screenshots (redacted)
└─ .gitignore

Example initial commit message
chore: initial commit — SOC L1 auth triage lab (mock logs + grep + escalation + cleanup)
