# File Polling Ports in webMethods

## Introduction to File Polling Ports in webMethods

A **File Polling Port** in **webMethods Integration Server** automatically checks a directory for new files and processes them when they arrive. It helps automate tasks like handling invoices, order files, or data transfers by monitoring and processing files as they are added to a folder.

Here’s how it works, step by step:

1. **Monitoring for New Files**:  
   - The server monitors a specific **Monitoring Directory** for new files.
   - It checks this directory at regular intervals to see if new files have arrived.

2. **Moving the File for Processing**:  
   - When a new file is found, the server moves it to a **Working Directory**.
   - The file is renamed with a unique ID so that it doesn’t conflict with other files.

   **Example**:  
   If a file named `invoice.txt` is found, it will be renamed to something like `FilePolling.001.invoice.txt`.

3. **Processing the File**:  
   - The server runs a **file processing service** on the file.
   - This service might perform tasks like parsing, validation, or transformation on the file's contents.

4. **Moving the File Based on Processing Outcome**:  
   - If the file is processed **successfully**, it is moved to the **Completion Directory**.
   - If the file processing **fails**, it is moved to the **Error Directory**.

   **Example**:  
   If the invoice file is processed successfully, it’s moved to the **Completion Directory**. If there’s an error (e.g., the file format is invalid), it is moved to the **Error Directory**.

5. **Cleaning Up Old Files**:  
   - The server automatically cleans up files in the Completion and Error directories after a certain time period.
   - You can set the server to delete old files to keep the directories clean.

6. **Handling Server Failures**:  
   - If the server shuts down before finishing a file’s processing, the file stays in the **Working Directory**.
   - When the server restarts, the administrator can decide whether to reprocess the file or delete it.

---

## Example Workflow:

1. **Set Up File Polling**:  
   - Configure the server to watch a folder (Monitoring Directory).
   
2. **File Detected**:  
   - The server finds a file (e.g., `invoice.txt`) in the Monitoring Directory.

3. **Rename and Move**:  
   - The file is renamed to `FilePolling.001.invoice.txt` and moved to the Working Directory.

4. **Processing**:  
   - A custom service (e.g., **processInvoice**) is executed to parse the file.

5. **Move Based on Result**:  
   - If the file is processed without issues, it goes to the **Completion Directory**.
   - If there’s an error (e.g., parsing issue), it goes to the **Error Directory**.

6. **Cleanup**:  
   - After a certain period, the server automatically deletes old files from the Completion and Error directories.

This process simplifies the automation of file handling, making it more efficient and reducing manual intervention.

---