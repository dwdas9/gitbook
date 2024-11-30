# How to Remove Spaces from OneDrive Folder Names Without Renaming

---

If you're a developer or a power user working with tools that don't handle spaces in folder names well, you may have run into issues with OneDrive folder names like `OneDrive - [Company Name]`. These names, automatically created by OneDrive, can break scripts, tools, or configurations that struggle with spaces. Here's a simple, non-destructive way to solve this problem using a **junction link**.

---

## What is a Junction Link?

A junction link is a type of symbolic link in Windows that acts like an alias for a folder. By creating a junction link, you can assign a custom, space-free name to your OneDrive folder without renaming it or editing the registry.

---

### Why Use a Junction Link?

- **Non-Destructive**: The original folder remains untouched.
- **Simple Setup**: No registry edits or system changes.
- **Reversible**: Easily delete the alias if you no longer need it.

---

### How to Create a Junction Link for Your OneDrive Folder

1. **Open Command Prompt as Administrator**:
   - Press `Win + S`, type "cmd", and select **Run as Administrator**.

2. **Navigate to Your OneDrive Parent Folder**:
   - Find the parent directory of your OneDrive folder. For example:

     ```text
     C:\Users\YourUsername
     ```

   - Navigate to this directory by typing:
  
     ```cmd
     cd C:\Users\YourUsername
     ```

3. **Create the Junction Link**:
   - Use the `mklink` command to create the alias. Replace `desired_name` with your custom name and `actual_name` with the full name of your OneDrive folder:
     
     ```cmd
     mklink /J desired_name "actual_name"
     ```
   
   - Example for a OneDrive folder named `OneDrive - MyCompany`:
     
     ```cmd
     mklink /J OneDriveMyCompany "OneDrive - MyCompany"
     ```

4. **Verify the Link**:
   - A new folder named `OneDriveMyCompany` will appear. It behaves just like the original folder.

---

### How to Use the Junction Folder
From now on, refer to the junction folder (`OneDriveMyCompany`) instead of the original folder (`OneDrive - MyCompany`). For example:
- In scripts:
  ```cmd
  cd C:\Users\YourUsername\OneDriveMyCompany
  ```
- In configuration files:
  ```ini
  output_dir = C:\Users\YourUsername\OneDriveMyCompany\Project
  ```

---

### Key Benefits
- **Ease of Use**: You don't need to modify your OneDrive settings or mess with the registry.
- **Compatibility**: Tools and scripts work seamlessly without worrying about spaces in folder names.
- **Flexibility**: The original folder name remains unchanged for OneDrive syncing.

---

### Conclusion
Using a junction link is a quick and efficient way to resolve issues caused by OneDrive folder names with spaces. It saves time, avoids unnecessary complexity, and works across all your tools and scripts. If you're a developer or power user, this simple tweak can make your life much easier!

