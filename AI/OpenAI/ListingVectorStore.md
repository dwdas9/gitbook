**How to Completely Remove Python 3.13 from Your Mac**

If you need to uninstall Python 3.13 from your Mac, follow these straightforward steps to ensure it's entirely removed:

1. **Download the Uninstall Script**

   - Open the Terminal application.

   - Enter the following command to download the uninstall script:

     ```bash
     curl -O https://raw.githubusercontent.com/csev/uninstall-python3/master/uninstall-python3.sh
     ```

2. **Review the Script**

   - Before running any script, it's essential to understand its actions. Open the script in a text editor to review its contents.

3. **Run the Script in Dry-Run Mode**

   - In Terminal, execute:

     ```bash
     bash uninstall-python3.sh
     ```

   - This will display the actions the script intends to perform without making any changes.

4. **Execute the Script with Administrative Privileges**

   - If you're satisfied with the dry-run output, run the script with superuser privileges:

     ```bash
     bash uninstall-python3.sh | sudo bash -v
     ```

   - You'll be prompted to enter your administrator password.

5. **Verify the Uninstallation**

   - After the script completes, check that Python 3.13 has been removed:

     - In Terminal, type:

       ```bash
       python3.13 --version
       ```

     - You should see a "command not found" message, indicating Python 3.13 is no longer installed.
