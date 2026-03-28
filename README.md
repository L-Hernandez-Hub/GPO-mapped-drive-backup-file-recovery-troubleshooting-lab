# Mapped Drive Backup & File Recovery Troubleshooting Lab


## Objective
Simulate a real-world IT support issue where a user reports that files from a mapped network drive are missing. Configure a shared folder and drive mapping through Group Policy, protect the data with Windows Server Backup, simulate file loss, recover the missing data, and verify that access is restored from the client side.

---

## Lab Environment
- Windows Server 2019 Virtual Machine
- Windows 11 Pro Virtual Machine
- Active Directory Domain Services (AD DS)
- Group Policy Management (GPO)
- Windows Server Backup
- Spiceworks Ticketing System

---

## Steps

### 1. Created the Shared Folder
Created a `Sales` shared folder on the server and added sample business files to simulate company data that users access through the network. 

![Screenshot 2](2-business-files.png)

---

### 2. Configured Network Share
Configured the Sales folder as a network share so it could be reached by users across the domain via Network Path: `\\HOME-LAB\Sales`.

![Screenshot 3](3-network-path.png)

---

### 3. Configured Drive Mapping Policy
Created a Group Policy to automatically map the Sales network share to the `S:` drive for users. Linked the drive mapping policy to the correct Organizational Unit `Sales_Users` so it would apply to the intended user account through Item-Level Targeting. 

![Screenshot 4](4-gpo-mapped-drive.png)

![Screenshot 5](5-item-level-targeting.png)

---

### 4. Verified Mapped Drive Access
Updated group policy using `gpupdate` on the client and confirmed that the S: drive appeared with the shared files available to the user.

**Command Used**
```
gpupdate /force
```

![Screenshot 6](6-gpupdate-file-access.png)

---

### 5. Installed and Configured Backup Protection
Installed Windows Server Backup under Add Roles and Features. A secondary virtual disk was created in UTM, formatted as NTFS, and assigned as `B:` to meet Windows Server Backup requirements for separate storage. Configured the backup and selected the dedicated destination drive to protect the shared `Sales` data.

![Screenshot 7](7-disk-mgmt.png)

![Screenshot 8](8-complete-backup.jpeg)

---

### 6. Confirmed Successful Backup
Confirmed the backup was successful by running the backup job and validating recoverability through the Recovery Wizard, ensuring shared folder data was accessible and protected prior to simulating the issue.

![Screenshot 9](9-confrimed-backup.png)

---

### 7. Simulated File Loss
Simulated a real support issue by removing the shared files and confirmed that the mapped drive no longer showed the expected data.

![Screenshot 10](10-files-lost.png)

---

### 8. Opened Support Ticket in Spiceworks
A user reports that all files from their mapped `S:` drive are missing. The drive is still mapped, but the shared business files are no longer available. Support ticket has been received.

![Screenshot 1](1-spiceworks-ticket.png)

---

### 9. Recovered Files from Backup
Used Windows Server Backup to recover the missing `Sales` files from the available backup set.

![Screenshot 11](11-file-recovery-sucess.png)

---

### 10. Verified File Recovery
Confirmed that the deleted files were restored on the server and became available again through the mapped `S:` drive on the client machine.

![Screenshot 12](12-business-files-confrimation-.png)

---

### 11. Closed Ticket After Resolution
Updated the support ticket with the recovery steps taken, verified the fix from the user side, and closed the case.

![Screenshot 13](13-ticket-resolution-verification.png)

![Screenshot 14](14-internal-note-steps.png)

---

## Key Takeaways
- Group Policy can automate drive mapping for users in the correct OU.
- Windows Server Backup can protect shared business data from accidental deletion.
- Verifying the fix from the client side is just as important as fixing it on the server.
- A complete troubleshooting workflow includes setup, failure confirmation, recovery, validation, and ticket closure.