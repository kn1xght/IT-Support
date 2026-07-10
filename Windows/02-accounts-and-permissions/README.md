# Local User Accounts & File Permissions

## Scenario

A local Windows user account was required to practice user administration and NTFS permission management in Windows 11. The goal was to create a temporary user, modify group membership, perform a password reset, configure NTFS permissions, verify folder access, and remove the temporary resources after testing.

Local user accounts can be administered through several Windows tools, including **Settings**, **Control Panel**, and **Local Users and Groups**. For this lab, `lusrmgr.msc` was used because it provides a more efficient interface for managing local users and group memberships.

## Environment

- **Operating system:** Windows 11 Enterprise Evaluation
- **Account type:** Local Windows user account
- **Administration tool:** Local Users and Groups (`lusrmgr.msc`)

## Skills Demonstrated

- Local user account creation
- Local group membership management
- Password reset 
- Local user account deletion
- NTFS permission configuration
- Permission inheritance management

## Implementation

### 1. Opened Local Users and Groups

The **Local Users and Groups** was opened by running `lusrmgr.msc`.

![Opened Local Users and Groups](screenshots/01-lusrmgr.msc.png)

### 2. Created a local user account

A temporary test account named `Marc` was created. The account was configured to require a password change at the next logon, allowing the user to set their own password privately.

![Created new local user](screenshots/02-create-new-local-user.png)

### 3. Verified the new user account

After creation, the new account appeared in the local users list. To make sure everything was configured correctly, the account's properties were reviewed.

![Verified new local user properties](screenshots/03-new-local-user-properties.png)

### 4. Added the user to the Administrators group

For demonstration purposes, the test account was added to the local `Administrators` group while retaining its default `Users` group membership. This granted the account rights to perform administrative tasks on the local machine. 

![Added local user to Administrators group](screenshots/04-local-user-to-administrator.png)

### 5. Reset the local user password

The password for the test account was reset to simulate a forgotten-password request, a common IT support scenario.

![Reset local user password](screenshots/05-local-user-password-reset.png)

### 6. Created a test folder

A folder named `Test Folder` was created on the `C:` drive for NTFS permission testing.

![Created test folder](screenshots/06-test-folder.png)

### 7. Created a test document

A document named `Test Document` was created inside the folder to verify file access after permissions were changed.

![Created test document](screenshots/07-test-document.png)

### 8. Disabled inherited permissions

Permission inheritance was disabled so the folder could be configured with explicit NTFS permissions.

![Disabled inherited permissions](screenshots/08-disable-inheritance.png)

### 9. Configured explicit folder permissions

Inherited permission entries were removed, leaving only the intended local account with access to the folder.

![Configured explicit folder permissions](screenshots/09-stanic-account-access-only.png)

### 10. Verified allowed access

The allowed account was able to open and edit the test document.

![Verified allowed access](screenshots/10-stanic-access-allowed.png)

### 11. Verified denied access

The restricted test account was unable to access the folder, confirming that the NTFS permissions were applied correctly.

![Verified denied access](screenshots/11-marc-access-denied.png)

### 12. Removed temporary user account and test folder

After the local account administration and NTFS permission testing was completed, the temporary user account `Marc` and the `Test Folder` directory were deleted.

![Removed temporary user account and test folder](screenshots/12-removed-temporary-account-and-folder.png)

## Result

The local user account `Marc` was created, assigned local administrator privileges, and used to simulate a password reset scenario. NTFS permissions were then configured on a test folder to control user access. Testing confirmed that `Stanic` could access the folder, including the document inside it, while `Marc` was denied access.

After the workflow was completed, the temporary user account and test folder were deleted.

## Note

Adding a user to the local `Administrators` group grants elevated privileges on the machine. In a real environment, this should be approached with caution, as unnecessary privileges can create security risks.

[← Return to Windows](../)