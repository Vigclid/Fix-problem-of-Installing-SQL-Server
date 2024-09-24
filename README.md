The error "**1639**, **Exit code Decimal**" when uninstalling SQL Server using the command `msiexec /x {GUID}` or via others method typically occurs due to issues related to the instance name or leftover installation files from previous install/uninstall attempts. Here are steps you can take to fix the issue:

### 1. Uninstall SQL Server Using SQL Server Setup
The best way to uninstall SQL Server is by using the **SQL Server Installation Center** instead of directly using the GUID:
1. Open **SQL Server Installation Center** from the Start Menu.
2. Select **"Remove SQL Server instance"** or **"Uninstall a product"**.
3. Select the instance you want to remove and follow the instructions to complete the uninstallation process.

### 2. Check and Delete Leftover Files and Folders
Sometimes, leftover SQL Server files and folders can cause issues during uninstallation:
1. **Delete leftover SQL Server directories**:
   - `C:\Program Files\Microsoft SQL Server`
   - `C:\Program Files (x86)\Microsoft SQL Server`
   - `C:\ProgramData\Microsoft\SQL Server`
2. **Clean up the registry**:
   - Open **Registry Editor** (`regedit`).
   - Navigate to the following registry keys and check if there are any remaining SQL Server entries:
     - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server`
     - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer`
   - Delete any keys related to the SQL Server instance you are trying to uninstall.
   
   **Note:** Always **backup the registry** before making any changes to avoid causing system issues.

### 3. Use **Microsoft Fix It Tool**
Microsoft has a tool called **Program Install and Uninstall Troubleshooter** that helps fix problems with installing or uninstalling programs. Follow these steps:
1. Download and run the tool from this link: [Microsoft Fix It](https://support.microsoft.com/en-us/topic/fix-problems-that-block-programs-from-being-installed-or-removed-85a7e888-29a4-41a5-a6d9-6b8b3b6e803c)
2. Run the program and select **SQL Server** from the list of installed programs to remove it.

### 4. Use `msiexec` to Uninstall Components Manually
If the previous methods don’t work, you can manually uninstall individual SQL Server **components**:
1. Open **Registry Editor** and navigate to:
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`
2. Find the GUIDs related to the SQL Server components.
3. Write down the GUIDs and run the following command for each one:
   ```bash
   msiexec /x {GUID}
   ```
   This will manually uninstall each SQL Server component.

### 5. Check for Running SQL Server Services
Make sure all SQL Server-related services are stopped before attempting to uninstall:
1. Open **Services** (`services.msc`).
2. Find all SQL Server-related services (e.g., **SQL Server (SQLEXPRESS)**, **SQL Server Browser**, etc.).
3. **Stop** all the SQL Server services that are still running.

### 6. Use `Setup.exe /ACTION=uninstall`
You can also use the SQL Server setup tool itself to uninstall from the command line:
1. Navigate to the SQL Server installation folder (e.g., `C:\SQL2022\Express_ENU\x64\setup\`).
2. Run the following command:
   ```bash
   setup.exe /ACTION=uninstall /INSTANCENAME=<instance_name>
   ```
   Replace `<instance_name>` with the name of the instance you want to uninstall, for example: `SQLEXPRESS`.

### 7. Remove Registered Entries via `WMIC`
You can use `WMIC` to check and remove installed SQL Server entries:
1. Open **Command Prompt** with **Administrator** privileges.
2. Enter the following command to list all installed SQL Server products:
   ```C#
   wmic product where "name like 'Microsoft SQL%'" get name
   ```
3. Uninstall a product by entering the following command for each entry:
   ```sql
   wmic product where "name='Microsoft SQL Server <version>'" call uninstall
   ```

### 8. Restart the System
After following all the steps above, **restart your computer** to complete the uninstallation process and clean up your system.

Once all steps are complete, you can try reinstalling SQL Server if needed.
