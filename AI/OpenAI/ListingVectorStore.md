- [Working with Vector Store Files in OpenAI: Listing and Deleting Failed Files](#working-with-vector-store-files-in-openai-listing-and-deleting-failed-files)
- [**Listing files and identifying failed uploads.**](#listing-files-and-identifying-failed-uploads)
- [**Deleting the failed files based on their status.**](#deleting-the-failed-files-based-on-their-status)
- [**Deleting all files in an organization**](#deleting-all-files-in-an-organization)

### Working with Vector Store Files in OpenAI: Listing and Deleting Failed Files

Here I will show you how to work with Vector store files in the OpenAI playground using Python. We will perform the followin activities:

1. Listing files and identifying failed uploads.
2. Deleting the failed files based on their status.
3. Deleting all files in your organization

All the codes have been built and tesetd throughly. They work 100%.

---

### **Listing files and identifying failed uploads.**

Here’s a Python script that lists all the files in a vector store and prints details about the ones with a status of `'failed'`:

```python
from openai import OpenAI
import os

def list_failed_files(vector_store_id):
    """
    Lists all files that failed to upload to a specified vector store.

    Args:
        vector_store_id: The ID of the vector store.
    """
    api_key = "your_openai_api_key"  # Replace with your API key

    # Instantiate the OpenAI client with your API key
    client = OpenAI(api_key=api_key)

    try:
        # Use the client to list files in the vector store
        vector_store_files = client.beta.vector_stores.files.list(
            vector_store_id=vector_store_id
        )

        # Iterate through the files and identify those with status 'failed'
        failed_files = [file for file in vector_store_files if file.status == 'failed']

        if failed_files:
            print(f"Found {len(failed_files)} failed files.")

            for file in failed_files:
                # Print details about the failed file
                print(f"  - ID: {file.id}")
                print(f"    Created at: {file.created_at}")
                print(f"    Error message: {file.last_error.message if file.last_error else 'None'}")

        else:
            print("No failed files found.")
    except Exception as e:
        print(f"An error occurred: {e}")

# Example usage:
vector_store_id = "your_vector_store_id"  # Replace with your actual vector store ID
list_failed_files(vector_store_id)
```

**Example Output**

If there are failed files, the output will look something like this:

```
Found 3 failed files.
  - ID: file-az9zboczxSUuPFSyuzdOYUyo
    Created at: 1729173761
    Error message: An internal error occurred.
  - ID: file-NobtIEx3O7jwQWirTiTaoU3Y
    Created at: 1729173761
    Error message: An internal error occurred.
  - ID: file-s5W9RE5cYpE4erUS02RIZ1fx
    Created at: 1729173761
    Error message: An internal error occurred.
```

---

### **Deleting the failed files based on their status.**

Once we have identified the failed files, the next step is to delete them from the vector store.

```python
from openai import OpenAI
import os

def list_and_delete_failed_files(vector_store_id):
    """
    Lists and deletes files that failed to upload to a specified vector store.

    Args:
        vector_store_id: The ID of the vector store.
    """
    api_key = "your_openai_api_key"  # Replace with your API key

    # Instantiate the OpenAI client with your API key
    client = OpenAI(api_key=api_key)

    try:
        # Use the client to list files in the vector store
        vector_store_files = client.beta.vector_stores.files.list(
            vector_store_id=vector_store_id
        )

        # Iterate through the files and identify those with status 'failed'
        failed_files = [file for file in vector_store_files if file.status == 'failed']

        if failed_files:
            print(f"Found {len(failed_files)} failed files. Deleting them...")

            for file in failed_files:
                # Print details about the failed file
                print(f"  - ID: {file.id}")
                print(f"    Created at: {file.created_at}")
                print(f"    Error message: {file.last_error.message if file.last_error else 'None'}")

                # Delete the failed file
                deleted_file = client.beta.vector_stores.files.delete(
                    vector_store_id=vector_store_id,
                    file_id=file.id
                )
                print(f"Deleted file: {deleted_file.id}")

        else:
            print("No failed files found.")
    except Exception as e:
        print(f"An error occurred: {e}")

# Example usage:
vector_store_id = "your_vector_store_id"  # Replace with your actual vector store ID
list_and_delete_failed_files(vector_store_id)
```

**Example Output**

```
Found 3 failed files. Deleting them...
  - ID: file-az9zboczxSUuPFSyuzdOYUyo
    Created at: 1729173761
    Error message: An internal error occurred.
Deleted file: file-az9zboczxSUuPFSyuzdOYUyo
  - ID: file-NobtIEx3O7jwQWirTiTaoU3Y
    Created at: 1729173761
    Error message: An internal error occurred.
Deleted file: file-NobtIEx3O7jwQWirTiTaoU3Y
  - ID: file-s5W9RE5cYpE4erUS02RIZ1fx
    Created at: 1729173761
    Error message: An internal error occurred.
Deleted file: file-s5W9RE5cYpE4erUS02RIZ1fx
```

### **Deleting all files in an organization**

This Python script connects to OpenAI's API, lists all files in your organization, and deletes them one by one. It uses the OpenAI Python client to automate the process, ensuring efficient cleanup of files.

```python
from openai import OpenAI

api_key = "yourkey"  # Ensure your API key is set as an environment variable

# Instantiate the OpenAI client with your API key
client = OpenAI(api_key=api_key)



def delete_all_files():
    """
    Deletes all files in the organization by listing and deleting each one.
    """
    try:
        # List all files in the organization
        files = client.files.list()

        # Iterate through the files and delete each one
        files_found = False
        for file in files:
            files_found = True
            file_id = file.id  # Access the file ID
            print(f"Deleting file: {file_id}")
            
            # Delete the file
            client.files.delete(file_id)
            print(f"File {file_id} deleted successfully.")

        if not files_found:
            print("No files found in the organization.")
        
    except Exception as e:
        print(f"An error occurred: {e}")

# Call the function to delete all files
delete_all_files()

```
