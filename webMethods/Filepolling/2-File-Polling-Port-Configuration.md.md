# Adding a File Polling Port in webMethods Integration Server

This document outlines the procedure to add a File Polling Port in webMethods Integration Server via the Administrator page.

## Steps to Add a File Polling Port

1. **Open the Integration Server Administrator**
   - If the Integration Server Administrator is not already open, navigate to it in your browser.

2. **Go to Ports Configuration**
   - In the Integration Server Administrator, go to the `Server` tab and select **Ports**.

   ![Server Ports Screenshot](path_to_your_screenshot)

3. **Add a New Port**
   - Click on the **Add Port** button.

4. **Select FilePolling Port Type**
   - From the list of port types, select **webMethods FilePolling**.

   ![Select FilePolling Port](path_to_your_screenshot)

5. **Submit the Port Type**
   - Click **Submit**. This will take you to a page where you can configure the port settings.

6. **Configure the Port Parameters**
   - **Package Name**: Enter the package associated with this port. For file handling, use **WmFlatFile**.
   - **Alias**: Provide a unique alias for the port (1-255 characters).
   - **Description**: Add a description for the port.

   Example configuration:
   - Package Name: `WmFlatFile`
   - Alias: `FilePollingPort1`
   - Description: `Port for monitoring incoming files`

   ![Configure Port Screenshot](path_to_your_screenshot)

7. **Configure Polling Information**
   - **Monitoring Directory**: Enter the directory to monitor for files.
   - **Working Directory (Optional)**: Directory where files will be moved for processing after being identified.
   - **Completion Directory (Optional)**: Directory where files will be moved once processing is complete.
   - **Error Directory (Optional)**: Directory for failed file processing.
   - **File Name Filter (Optional)**: Enter a filter to specify the types of files to be processed.
   - **File Age (Optional)**: Set the minimum age (in seconds) for files before they are processed.
   - **Content Type**: Specify the content type (e.g., `text/xml`).
   - **Allow Recursive Polling**: Choose whether to poll sub-directories.
   - **Enable Clustering**: Choose whether to allow clustering.

   Example:
   - Monitoring Directory: `/data/incoming/`
   - Working Directory: `/data/processing/`
   - File Name Filter: `*.txt`
   - Content Type: `text/xml`
   
   ![Polling Information Screenshot](path_to_your_screenshot)

8. **Set Security and User**
   - In the **Run services as user** field, select the user for running the service assigned to the port.

9. **Configure Message Processing**
   - **Enable**: Choose `Yes` to enable the port.
   - **Processing Service**: Enter the service name to execute when a file is processed.
   - **File Polling Interval**: Set the polling interval (in seconds).
   - **Log Only When Directory Availability Changes**: Choose whether to log availability changes.
   
   Example:
   - Processing Service: `processFile`
   - Polling Interval: `10`
   
   ![Message Processing Screenshot](path_to_your_screenshot)

10. **Save Changes**
    - After configuring all necessary settings, click **Save Changes**.

11. **Set Access Mode for the Port**
    - **Edit the Access Mode**: Click **Edit** in the Access Mode field.
    - Set the access mode to **Deny by Default**.
    - Add the processing service to the **Allow List**:
        - Type the service name in the text box.
        - Remove any other services from the list.

    Example:
    - Access Mode: `Deny by Default`
    - Allowed Service: `processFile`

    ![Access Mode Screenshot](path_to_your_screenshot)

---

### Final Note:
If you change the processing service, remember to update the Allow List accordingly.

---

You can place your screenshots in the appropriate locations by replacing `path_to_your_screenshot` with the actual file path or URL where the images are stored. This structure ensures that each step is clear and the necessary screenshots are included for better understanding.