# Smartsheet API Folder Monitoring Application

## An automation utility that monitors a local directory to detect new files, extract data from filenames, and send the extracted data directly to a Smartsheet spreadsheet.

## Features
* **Folder Monitoring:** Tracks a user-selected local directory for newly added files.
* **Data Extraction:** Automatically parses the employee's first name and quote number from Word document filenames.
* **Instant Smartsheet Updates:** Transmits extracted data directly to your sheet via the Smartsheet API without manual entry.
* **Error Handling:** Validates folder paths and ensures stable API connectivity during data transfer.

## Live Demo
[![Smartsheet API folder monitoring GUI app](https://github.com/JasonDev546/JC-Automation-/blob/main/SmartsheetGUIAPP.jpg?raw=true)](https://www.youtube.com/watch?v=O7L47zKL2pU)

## Prerequisites

Before running the application, ensure you have the following: 

* **Python 3.8+** installed in your system.
* **Register and Log in** to your Smartsheet account
* A **Smartsheet Access Token** (generated under *Account > Personal Settings > API Access > Generate new access token*) **Payment is required**.
* The **Sheet ID** of the target Smartsheet spreadsheet.

## Installation

1. Clone this repository to your local machine:
   ```bash
   git clone https://github.com/JasonDev546/JC-Automation-
   cd JC-Automation-
   ``` 
2.  Install the necessary Python packages required for the script to execute successfully:
```
pip install smartsheet-python-sdk
pip install requests
```

## Configuration
The application requires your unique Smartsheet access details to push data. Two files are used to include this data, which are the following: FILEMONITORING_GUI.py and SMARTSHEET_API_REQUESTS.py.

```python
# FILEMONITORING_GUI.py
self.api_token = "YOUR_ACTUAL_API_TOKEN_HERE"
self.sheet_id = "YOUR_TARGET_SHEET_ID" 


# SMARTSHEET_API_REQUESTS.py
api_token = "YOUR_ACTUAL_API_TOKEN_HERE"
```

## Usage
1. Configure your Smartsheet API token and Sheet ID.
2. Start the application.
3. Select the folder you want to monitor.
4. Add a new Word document (`.docx`) to the monitored folder.
5. The filename must use an underscore (`_`) to separate the employee's first name from the quote number, and the quote number must begin with `Q`.
6. The application uses everything before the final underscore as the employee's first name and the final portion as the quote number.
7. For example, a file named `John_Q12345.docx` is interpreted as:
   - **First Name:** John
   - **Quote #:** Q12345
8. The extracted first name and quote number are automatically sent to the configured Smartsheet.
9. The `.xlsx` files are detected and reported by the application but they are not used for data extraction.
10. Other file types are reported as invalid.

    
## Dependencies

### External Packages
* requests
* smartsheet-python-sdk

### Python Standard Library
* tkinter
* threading
* os

## Design

[UML Use Case Diagram](https://github.com/JasonDev546/JC-Automation-/blob/main/UMLUseCaseDiagram.png)

In this UML diagram, the primary actor is the “Employee,” as it is the company employee who initiates the use of this application. The secondary actors are the “File System” and the “Smartsheet API,” both of which react automatically during the application’s execution. The employee begins using the application by entering the path to the relevant folder and then clicking the “Start Monitoring” button within the Tkinter window. There is an association relationship between the employee and the action of starting the monitoring process. Each time the user clicks this button, the folder path is verified to ensure it is valid; consequently, there is an <<include>> relationship between “Start Monitoring Folder” and “Verify Folder Path.” Furthermore, there is an <<extend>> relationship between “Display Folder Path Error” and “Start Monitoring Folder,” as there may be instances where the employee enters an incorrect path for the folder they wish to monitor. If this occurs, a window appears indicating that the folder path is invalid. Once monitoring of the folder begins, data is retrieved from the file system, which explains the association relationship between the “File System” and “Start Monitoring Folder.” Since the employee can stop monitoring a folder by clicking the “Stop Monitoring” button in the Tkinter window, there is an association relationship between the primary actor and the action of “Stop Monitoring Folder.” Since an employee can click on “Change Folder Path,” and the “File System” can respond by opening a window that allows the user to browse for the path to the folder they wish to monitor, there is an association relationship between “File System” and “Change Folder Path,” as well as an association relationship between “Employee” and “Change Folder Path.” There is a generalization relationship regarding the action of adding a file to the monitored folder, because the added file may or may not follow the naming convention that combines the employee’s first name with their corresponding quote number. If the file follows this naming convention, the employee’s name and quote number—extracted from the filename—are automatically inserted into a Smartsheet spreadsheet via its API. This process is triggered every time a file with this specific naming convention is added; this explains the <<include>> relationship between “Add file with valid naming convention” and “Insert the employee name and the quote number from the valid filename into the Smartsheet spreadsheet via its API.” Within the latter action, the necessary API information is provided to connect the application to Smartsheet and perform the insertion, which accounts for the association relationship between that action and the “Smartsheet API.” Conversely, if a file added to the monitored folder does not follow this convention, a message is displayed in the message box within the main Tkinter window, indicating that an invalid file has been added to the folder. Since this always executes when the added file does not follow that convention, there is another <<include>>-type relationship between “Add file without valid name convention” and “Display invalid file has been added message.”

## Project structure

```text
JC-Automation-/
├── FILEMONITORING_GUI.py        # Main GUI and folder monitoring application
├── SMARTSHEET_API_REQUESTS.py   # Handles Smartsheet API requests
├── requirements.txt             # Python package dependencies
├── README.md                    # Project documentation
├── .gitignore                   # Specifies files Git should ignore
├── SmartsheetGUIAPP.jpg         # Screenshot of the application
├── UMLUseCaseDiagram.png        # Screenshot of the UML Use Case Diagram
├── LICENSE                      # MIT License
```

## Licensing
This project is licensed under the [MIT License](https://opensource.org/license/mit).

