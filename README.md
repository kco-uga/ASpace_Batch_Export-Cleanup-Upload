# ArchivesSpace Batching
##Summary
This application batch exports records from ArchivesSpace in EAD, MARCXML, Container Label, or PDF form. 
Additionally, it can run exported EAD records through a series of cleanup processes. Lastly, a user can
download the XTF version to upload .xml or .pdf files to their instance of XTF.

## Requirements
1. Python 3 installed on your computer. You can install python using the following link:
https://www.python.org/downloads/
2. Your ArchivesSpace Instance's API URL (8089), XTF hostname URL, XTF remote path for EAD files, and XTF indexer
path to re-index new and/or changed files.

## Process
This is a walkthrough of the program and how it functions.

1. It takes resource identifiers seperated by new lines or commas from ArchivesSpace as inputs, searches
the ArchivesSpace database, exports EAD, MARCXML, Container Labels, or PDF files for the resources
specified, cleans the EAD.xml files, and saves them locally.
2. For XTF users, EAD.xml or .pdf files as selected by the user are uploaded to the 
XTF finding aid website/database and the program indexes just those records
uploaded/changed.
### Login Windows
Upon running the program, the user is met with 2 windows:
1. ArchivesSpace login credentials - input your username, password, and the ArchivesSpace
API URL.
2. XTF login credentials - input your username, password, the XTF Hostname and XTF Remote
Path (where the EAD files are stored).
### EAD Screen
On the top right contains 4 radio buttons where users can select
what export option they would like to use. The default is set to EAD. Addtionally, a user must select which 
repository they would like to export from. The default is Search Across Repositories, but you can save a different
repository clicking on the "Save Repository as Default" button. 


There are 3 sections of options:
1. Export - When a user inputs a resource identifier in the multiline text import 
(Large Box #1) listing them in either newlines or seperated by commas, this button will search the 
ArchivesSpace database for resource identifiers that match the inputs. It will then take those it found and
 run them through a cleanup script, if set by EAD Export Options, and save them to a local folder called 
 clean_eads by default.

    1. Open Raw ASpace exports (Optional Parameter) - Checking this will save the EAD.xml
    files as exported by ArchivesSpace and open a file explorer window in a folder called
    source_eads. These EAD.xml files are NOT run through cleanup.py script.
2. Options:
    1. EAD Export Options - This button allows the user to customize how they would like the EAD records to 
    be exported. This includes setting the options to keep the raw ASpace exports and running the cleanup
    process on the exported EAD.xml records.
    2. Cleanup Options - This button allows the user to select what sorts of cleanup they would like the 
    EAD record to run through. The default is set to all.
3. Output:
    1. Open Cleaned EAD Folder - This button opens the folder where EAD records that have gone through the
    cleanup process are saved. You can customize this folder in EAD Export Options.
    2. Open Raw ASpace Exports - If a user has selected "Keep RAW ASpace Exports" in EAD Export Options, 
    this button will open the folder where the raw ASpace EAD records are saved. You can customize this
    folder in the EAD Export Options
### MARCXML Screen
On the top right contains 4 radio buttons where users can select
what export option they would like to use. The default is set to EAD. Addtionally, a user must select which 
repository they would like to export from. The default is Search Across Repositories, but you can save a different
repository clicking on the "Save Repository as Default" button.


There are 2 options:
1. EXPORT - When a user inputs a resource identifier in the multiline text import 
(Large Box #1) listing them in either newlines or seperated by commas, this button will search the 
ArchivesSpace database for resource identifiers that match the inputs. It will return the files according
to the default directory or one specified by the user.
2. Options:
    1. MARCXML Export Options - allows user to specify options on how to export their MARCXML and what folder
    they want the records saved in.
    2. Open Output - open the folder where MARCXML files are being exported to.

###Container Labels Screen
On the top right contains 4 radio buttons where users can select
what export option they would like to use. The default is set to EAD. Addtionally, a user must select which 
repository they would like to export from. The default is Search Across Repositories, but you can save a different
repository clicking on the "Save Repository as Default" button.


There are 2 options:
1. EXPORT - When a user inputs a resource identifier in the multiline text import 
(Large Box #1) listing them in either newlines or seperated by commas, this button will search the 
ArchivesSpace database for resource identifiers that match the inputs. It will return the files according
to the default directory or one specified by the user.
2. Options:
    1. Open Output - open the folder where MARCXML files are being exported to.
    2. Choose Output Folder: - Choose the output folder where you would like container labels to be exported
    to.
 
###PDF Screen
!WARNING! - This feature is only compatible with ArchivesSpace version 2.8.0 or greater. The program will
tell you what version you are running on the WARNING label in this screen.


On the top right contains 4 radio buttons where users can select
what export option they would like to use. The default is set to EAD. Addtionally, a user must select which 
repository they would like to export from. The default is Search Across Repositories, but you can save a different
repository clicking on the "Save Repository as Default" button.


There are 2 options:
1. EXPORT - When a user inputs a resource identifier in the multiline text import 
(Large Box #1) listing them in either newlines or seperated by commas, this button will search the 
ArchivesSpace database for resource identifiers that match the inputs. It will return the files according
to the default directory or one specified by the user.
2. Options:
    1. PDF Export Options - This button allows the user to customize how they would like the PDF records to 
    be exported. This includes setting the output folder for exports.
    2. Open Output - open the folder where MARCXML files are being exported to.

## Testing
Right now, the best way to test the program is to input resource identifiers and try uploading
them to XTF. Some examples of Hargrett and Russell resource identifiers include:

* ms697
* ms905 - biggest one, will take a long time to export and index
* ms2929
* RBRL/044/CFH
* RBRL/112/JRR
* RBRL/109/JHH
* RBRL/067/HEM

If you want to generate errors, input any string or random numbers, such as "hello world"
or 42
## Code Structure
### as_xtf_GUI.py
This file acts as the Graphics User Interface (GUI) for the program. It uses 
PySimpleGUI and its dependencies. There are _5_ functions in the program.
#### run_gui()
There's a lot to this function, so get ready for a wild and long ride.... This function 
handles the GUI operation as outlined by PySimpleGUI's guidelines. There are 2 components
to it, the setup code and the _while_ loop.
##### Setup Code
`sg.ChangeLookAndFeel()` changes the theme of the GUI (colors, button styles, etc.).
`as_username`, `as_password`, `as_api`, `close_program_as`, `client`, `version`, `repositories`
`cleanup_default` and `cleanup_options` store the options if a user wants to specify what 
cleanup operations to run through cleanup.py.
`menu_def` sets the options for the toolbar menu.
`ead_layout`, `xtf_layout`, `marc_layout`, `contlabel_layout`, `pdf_layout` 
all set the various layouts for their respective screens.
`simple_layout_col1` and `simple_layout_col2` divide the layout into two columns, the first being defined with the
multiline input for resource identifiers, the second defining the above export layouts and Output console.
`layout_simple` sets the layout for the GUI.
`window_simple` sets the window for the main screen. Any variable with _window follows the
same logic.
##### while loop
The way the loop works is that the program "reads" the Window we made above in the setup code
and returns events and values (`event_simple` and `values_simple`). If a user clicks a button
with a specific key as outlined in the layout, that is acted upon under the following
`if` statements.

There are _28_ `if` statements in the while loop:
1. `if event_simple == 'Cancel' or event_simple is None or event_simple == "Exit":` - This exits the program 
for the user.
2. `if event_simple == "_EXPORT_EAD_RAD_":` This sets the screen to the EAD layout
3. `if event_simple == "_EXPORT_MARCXML_RAD_":` This sets the screen to the MARCXML layout
4. `if event_simple == "_EXPORT_PDF_RAD_":` This sets the screen to the PDF layout
5. `if event_simple == "_EXPORT_CONTLABS_RAD_":` This sets the screen to the Container Label layout
6. `if event_simple == "_REPO_DEFAULT_":` This is the 'Save Repository as Default' button, which saves the 
user's chosen repository as default in defaults.json.
7. `if event_simple == "_EXPORT_EAD_":` - This activates searching ASpace, exporting, and 
cleaning the EAD.xml files. Under this are `if values_simple["_OPEN_RAW_"] is True:`, which
checks if the user selected the "Open raw ASpace exports" checkbox.
8. `if event_simple == "_EAD_OPTIONS_" or event_simple == "Change EAD Export Options":` This is the 'EAD Export
Options' button.
9. `if event_simple == "_OPEN_CLEAN_B_" or event_simple == 'Open Cleaned EAD Folder':` - This is the 'Open
Cleaned EAD Folder' button. Opens clean_eads folder. Outlined in the toolbar menu
10. `if event_simple == "_OPEN_RAW_EXPORTS_" or event_simple == "Open RAW ASpace Exports":` - This is the
'Open Raw ASpace Exports' button. Opens the source_eads folder. Outlined in the toolbar menu and button
11. `if event_simple == "_EXPORT_MARCXML_":` This activates searching ASpace, exporting, and placing 
MARCXML files in the default folder or folder specified by the user
12. `if event_simple == "_OPEN_MARC_DEST_":` This is the 'Open Output' button and will open the default
MARCXML folder or folder as specified by the user
13. `if event_simple == "_MARCXML_OPTIONS_" or event_simple == "Change MARCXML Export Options":` - This is the
button 'MARCXML Export Options'
14. `if event_simple == "_EXPORT_PDF_":` - This activates searching ASpace, exporting, and placing 
PDF files in the default folder or folder specified by the user
15. `if event_simple == "_OPEN_PDF_DEST_":` - This is the 'Open Output' button and will open the default
MARCXML folder or folder as specified by the user
16. `if event_simple == "_PDF_OPTIONS_" or event_simple == "Change PDF Export Options":` - This is the
button 'PDF Export Options'
17. `if event_simple == "_EXPORT_LABEL_":` - This activates searching ASpace, exporting, and placing 
container label (.tsv) files in the default folder or folder specified by the user
18. `if event_simple == "_OPEN_LABEL_DEST_":` - This is the 'Open Output' button and will open the default
container label folder or folder as specified by the user
19. `if event_simple == "_OUTPUT_DIR_LABEL_INPUT_":` - This is the folder where container label files are exported
20. `if event_simple == "Change ASpace Login Credentials":` - runs the function 
`get_aspace_log()` to set ArchivesSpace login credentials. Outlined in the toolbar menu 
(Edit or File/Settings)
21. `if event_simple == 'Change XTF Login Credentials':` - runs the function `get_xtf_log()`
to set XTF login credentials. Outlined in the toolbar menu (Edit or File/Settings)
22. `if event_simple == "Clear Cleaned EAD Folder":` - deletes all files in clean_eads. 
Outlined in the toolbar menu (File)
23. `if event_simple == "Clear Raw ASpace Export Folder":` - deletes all files in 
source_eads. Outlined in the toolbar menu (File)
24. `if event_simple == "Change Cleanup Defaults":` - activates a popup (window) where users
can change the operations of cleanup.py script. Still working on this (should set all to
true and let users uncheck options??). Outlined in the toolbar menu (Edit or File/Settings)
25. `if event_simple == "About":` - displays a popup (really another window) describing info
about the program version. Users can select the Check Github button, which will open a 
browser tab to the Github version page for the program. Outlined in the toolbar menu (Help)

26. `if event_simple == "_UPLOAD_":` - clicking the button Upload, opens the popup 
(window) for users to upload to XTF. Links out to xtf_upload.py to upload and execute 
an indexing of the most recent files to be uploaded
27. `if event_simple == "_INDEX_":` - This button re-indexes the XTF hostname specified by using a -index default.
This will NOT do a clean index, but only indexing the any changed or new files added to the XTF remote path
28. `if event_simple == "_XTF_OPTIONS_":` This button opens a popup letting the user modify XTF upload and indexing
options
#### get_aspace_log()
##### Parameters
1. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default

##### Returns
1. as_username
2. as_password
3. as_api
4. close_program
5. client
6. version
7. repositories


This function gets a user's ArchiveSpace credentials. There are 3 components
to it, the setup code, correct_creds while loop, and the window_asplog_active while loop.
##### Setup Code
    as_username = None # ArchivesSpace credentials to to be set later in the function
    as_password = None # ArchivesSpace credentials to to be set later in the function
    as_api = None
    client = None
    repositories = {"Search Across Repositories (Sys Admin Only)": None}
    window_asplog_active = True  # To help break from the login verification while loop
    correct_creds = False # To help break out of the login verification while loop
    close_program = False # If a user hits the X button, it passes that on to the 
    # run_gui() function which closes the program entirely.
##### while correct_creds is False
Yes, I know it's weird. I need to change this. This while loop keeps the ArchivesSpace
login window open until a user fills in the correct credentials and sets `correct_creds`
to True.

It sets the columns for the layout, the layout, and the window.
##### while window_asplog_active is True
Again, I know I could leave off the "is True" part. Just trying to be explicit. The way 
the loop works is that the program "reads" the Window we made above in the setup code and 
returns events and values (event_log and values_log). The window takes the inputs 
provided by the user using `as_username = values_log["_ASPACE_UNAME_"]` for instance.

The try, except statement tries to authorize the user using the ArchivesSpace ASnake
client, which is a package for handling API requests to ArchivesSpace. If it fails to 
authenticate, a popup is generated saying the credentials are incorrect. The cycle continues
until the credentials are correct, or the user exits out of the login window.
#### get_xtf_log()
##### Parameters
1. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default

##### Returns
1. xtf_username
2. xtf_password
3. xtf_host
4. xtf_remote_path
5. xtf_indexer_path
6. close_program


This function gets a user's XTF credentials. There are 3 components to it, the setup code,
correct_creds while loop, and window_xtflog_active while loop.
##### Setup Code
    xtf_username = None # XTF credentials to to be set later in the function
    xtf_password = None # XTF credentials to to be set later in the function
    xtf_host = None
    xtf_remote_path = None 
    xtf_indexer_path = None 
    window_xtflog_active = True # To help break out of the GUI while loop
    correct_creds = False # To help break from the login verification while loop
    close_program = False # If a user hits the X button, it passes that on to the
    # run_gui() function which closes the program entirely.
##### while correct_creds is False
This while loop keeps the ArchivesSpace login window open until a user fills in the correct
 credentials and sets `correct_creds` to True.

It sets the columns for the layout, the layout, and the window.
##### while window_asplog_active is True
The way the loop works is that the program "reads" the Window we made above in the setup 
code and returns events and values (`event_xlog` and `values_xlog`). The window takes the 
inputs provided by the user using `xtf_username = values_xlog["_XTF_UNAME_"]` for instance.

The try, except statement tries to authorize the user by checking the class variable 
`self.scp` in xtf_upload.py. This is only set if the connection to XTF is successful, found
in the function `connect_remote(self)` in xtf_upload.py. If it fails to authenticate, 
a popup is generated saying the credentials are incorrect. The cycle continues
until the credentials are correct, or the user exits out of the login window.
#### get_eads()
##### Parameters
1. input_ids - a list of user inputs as gathered from the Resource Identifiers input box
2. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default
3. cleanup_options - a list of options a user wants to run against an EAD.xml file after export to clean the file
4. repositories - a dictionary of repositories as listed in the ArchivesSpace instance
5. client - a client object from ASnake.client to allow to connect to the ASpace API
6. values_simple - a list of values as entered with the `run_gui()` function


This function iterates through the user input in the Resource Identifier text box on the left side of the screen and
sends them to as_export.py to `fetch_results()` and `export_ead()`.

It first checks for commas or newlines in the Resource Identifier box and splits the input and adds it to a list
called `resources`.

##### for input_id in resources:
The for loop begins by iterating through each user input as created in the resources list above. 
It then initializes a class instance of ASExport from as_export.py and runs the `fetch_results()`
function on each input. If an error occurs when fetching results, the class instance `self.error` will not be none
and an if-else statement will default to else, printing the error statement in the Output Terminal.

If there are results that both match and do not match exactly the input, those that did not match but were fetched
are added to `self.result` and printed.

For the singular result that matched, it is run against the class method `export_ead()`. If any errors are caught, the
if-else statement will determine if `self.error` is not none and print the error to the Output terminal. If no error
is detected, it will ask whether or not the user selected to run cleanup.py on the exported records and if the user
wants to keep the raw ASpace EAD exports.

#### get_ead_options()
##### Parameters
1. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default

This function opens a window in the GUI that allows a user to choose specific export options. These options include:
   1. Include unpublished components (default is false)
   2. Include digital objects (default is true)
   3. Use numbered container levels (default is true)
   4. Convert to EAD3 (default is false)
   5. Keep raw ASpace Exports (default is false)
   6. Set raw ASpace output folder
   7. Clean EAD records on export (default is true)
   8. Set clean ASpace output folder
   
The function will write the options selected to the defaults.json file. Additionally, a user is alerted if Keep raw
ASpace Exports and Clean EAD records on export are set to false - the result of which will mean no 
file will be kept on export.

#### get_cleanup_defaults()
##### Parameters
1. cleanup_defaults - a list of all the default values a user can select for cleaning an EAD.xml file
2. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default

This function opens a window in the GUI that allows a user to choose what operations they want to run in order to
clean any exported EAD.xml files. These options include:
   1. Add Resource ID as EADID - Takes the resource identifier as listed in ArchivesSpace and
   copies it to the <eadid> element in the EAD.xml file.
   2. Delete Empty Notes - Searches for every `<p>` element in the EAD.xml file and 
   checks if there is content in the element. If not, it is deleted.
   3. Remove Non-Alphanumerics and Empty Extents - Does 2 things. It deletes any empty `<extent>` elements and 
   removes non-alphanumeric characters from the beginning of extent elements. An example would be: 
   `<extent>(13.5x2.5")</extent>`. This would change to `<extent>13.5x2.5"</extent>`.
   4. Add Certainty Attribute - Adds the attribute `certainty="approximate"` to all dates
   that include words such as circa, ca. approximately, etc.
   5. Add label='Mixed Materials - Adds the attribute `label="Mixed Materials"` to any container
   element that does not already have a label attribute.
   6. Delete Empty Containers - Searches an EAD.xml file for all container elements
   and deletes any that are empty.
   7. Add Barcode as physloc Tag - This adds a `physloc` element to an element when a container has 
   a label attribute. It takes an appended barcode to the label and makes it the value of the physloc tag.
   8. Remove Archivists' Toolkit IDs - Finds any unitid element with a type that includes an 
   Archivists Toolkit unique identifier. Deletes that element.
   9. Remove xlink Prefixes from Digital Objects - Counts every attribute that occurs in a `<dao>` element.
   10. Remove Unused Namespaces - Removes any unused namespaces in the EAD.xml file.
   11. Remove All Namespaces - Replaces other namespaces not removed by `clean_unused_ns()` in the `<ead>` 
   element with an empty `<ead>` element.

The function will write the options selected to the defaults.json file.

#### get_marcxml()
##### Parameters
1. input_ids - a list of user inputs as gathered from the Resource Identifiers input box
2. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default
3. repositories - a dictionary of repositories as listed in the ArchivesSpace instance
4. client - a client object from ASnake.client to allow to connect to the ASpace API
5. values_simple - a list of values as entered with the `run_gui()` function

This function iterates through the user input in the Resource Identifier text box on the left side of the screen and
sends them to as_export.py to `fetch_results()` and `export_marcxml()`.

It first checks for commas or newlines in the Resource Identifier box and splits the input and adds it to a list
called `resources`.

##### for input_id in resources:
The for loop begins by iterating through each user input as created in the resources list above. 
It then initializes a class instance of ASExport from as_export.py and runs the `fetch_results()`
function on each input. If an error occurs when fetching results, the class instance `self.error` will not be none
and an if-else statement will default to else, printing the error statement in the Output Terminal.

If there are results that both match and do not match exactly the input, those that did not match but were fetched
are added to `self.result` and printed.

For the singular result that matched, it is run against the class method `export_marcxml()`. If any errors are caught,
the if-else statement will determine if `self.error` is not none and print the error to the Output terminal.

#### get_marc_options()
##### Parameters
1. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default

This function opens a window in the GUI that allows a user to choose specific export options. These options include:
   1. Include unpublished components (default is false)
   2. Open output folder on export (default is false)
   3. Set output folder
   
The function will write the options selected to the defaults.json file.

#### get_pdfs()
##### Parameters
1. input_ids - a list of user inputs as gathered from the Resource Identifiers input box
2. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default
3. repositories - a dictionary of repositories as listed in the ArchivesSpace instance
4. client - a client object from ASnake.client to allow to connect to the ASpace API
5. values_simple - a list of values as entered with the `run_gui()` function

This function iterates through the user input in the Resource Identifier text box on the left side of the screen and
sends them to as_export.py to `fetch_results()` and `export_pdf()`.

It first checks for commas or newlines in the Resource Identifier box and splits the input and adds it to a list
called `resources`.

##### for input_id in resources:
The for loop begins by iterating through each user input as created in the resources list above. 
It then initializes a class instance of ASExport from as_export.py and runs the `fetch_results()`
function on each input. If an error occurs when fetching results, the class instance `self.error` will not be none
and an if-else statement will default to else, printing the error statement in the Output Terminal.

If there are results that both match and do not match exactly the input, those that did not match but were fetched
are added to `self.result` and printed.

For the singular result that matched, it is run against the class method `export_pdf()`. If any errors are caught,
the if-else statement will determine if `self.error` is not none and print the error to the Output terminal.

#### get_pdf_options()
##### Parameters
1. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default

This function opens a window in the GUI that allows a user to choose specific export options. These options include:
   1. Include unpublished components (default is false)
   2. Include digital objects (default is true)
   3. Use numbered container levels (default is true)
   4. Convert to EAD3 (default is false)
   2. Open ASpace Exports on Export (default is false)
   3. Set output folder
   
The function will write the options selected to the defaults.json file.

#### get_contlabels()
##### Parameters
1. input_ids - a list of user inputs as gathered from the Resource Identifiers input box
2. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default
3. repositories - a dictionary of repositories as listed in the ArchivesSpace instance
4. client - a client object from ASnake.client to allow to connect to the ASpace API
5. values_simple - a list of values as entered with the `run_gui()` function

This function iterates through the user input in the Resource Identifier text box on the left side of the screen and
sends them to as_export.py to `fetch_results()` and `export_labels()`.

It first checks for commas or newlines in the Resource Identifier box and splits the input and adds it to a list
called `resources`.

##### for input_id in resources:
The for loop begins by iterating through each user input as created in the resources list above. 
It then initializes a class instance of ASExport from as_export.py and runs the `fetch_results()`
function on each input. If an error occurs when fetching results, the class instance `self.error` will not be none
and an if-else statement will default to else, printing the error statement in the Output Terminal.

If there are results that both match and do not match exactly the input, those that did not match but were fetched
are added to `self.result` and printed.

For the singular result that matched, it is run against the class method `export_labels()`. If any errors are caught,
the if-else statement will determine if `self.error` is not none and print the error to the Output terminal.

#### get_xtf_options()
##### Parameters
1. defaults - a dictionary containing the data from defaults.json file, all data the user has specified as default

This function allows a user to select what options they want when uploading and re-indexing records in XTF. These
options include:
   1. Re-index changed records upon upload (default is true)
   2. Select source folder
   3. Change XTF Login Credentials - button that opens the XTF popup window

The function will write the options selected to the defaults.json file.

#### open_file()
##### Parameters
1. filepath - a filepath as input

This function takes a filepath and opens the folder according to whichever Operating system the user is using including
Windows, Mac, and Linux.

#### fetch_local_files()
##### Parameters
1. local_file_dir - the local file directory path used to determine what file to use for uploading to XTF
2. select_files - a list of files to be uploaded to XTF

This function creates a list of the files to be uploaded to XTF, as written by Todd Birchard in his article
'SSH & SCP in Python with Paramiko', https://hackersandslackers.com/automate-ssh-scp-python-paramiko/
#### if __name__ == "__main__"
main checks for directories in the current directory the GUI or .exe is located. If one of the folders is not found,
it runs the setup.py file. It also tries to open the defaults.json file and load it. If it does not work, it will
run either the xtf `set_default_file_xtf()` or non-xtf `set_default_file()` setup.py functions. It will proceed to
run the `run_gui()` function with the json_data loaded from defaults.json.

### as_export.py
This script searches the ArchivesSpace database for a user-input resource identifier and 
exports an EAD.xml file of the resource if found. There are 2 functions in the script.

The script executes a try, except clause searching for the existance of a folder called
source_eads. If it does not find the folder, it will generate it at the same level as
the script.
#### fetch_results()
##### Parameters:
1. input_id - accepts 1 resource identifier at a time (str)
2. as_username - (str)
3. as_password - (str)
4. as_api - (str)

This function takes a resource indentifier as input by the user in the GUI and searches 
ArchivesSpace for a resource that matches the identifier. If it matches with a resource,
it then extracts the resource URI and the resource repository number and returns them. If
an error occurs in any step of the process, a tuple is returned with None as the first
return and a string detailing the error.

The steps for the this function are as follows:
1. Assign `resource_uri` and `resource_repo` to empty strings.
2. Try, Except to initiate an ASnake client. ArchivesSnake is a package to help work with
the ArchivesSpace API. Assign the client as variable `client`.
3. If the input contains either a - or . in it, split the id and add each part to the list
 `id_lines`.
 4. Perform a search using client.get (similar to request.get).
    1. In the ArchivesSpace API, we use the endpoint /search to search across the entire 
    instance. https://archivesspace.github.io/archivesspace/api/#search-this-archive
    2. Parameters:
        1. `"q": 'four_part_id:' + input_id` - q is the optional search query (str), we 
        include "four_part_id:" as that narrows our search to looking for the complete
        resource identifier. We add the input_id as a whole.
        2. `"page": 1` - this is the only option so far, it will return 10 results if 
        the user input returns multiple results. I need to change this to `get_paged`
        and account for returning more than 10 results. So far, I haven't returned more
        than 10.
        3. `"type": ['resource']` - this specifies that we are looking for resources only
 5. We check the status code of the value of `search_resources`. If it is not 200, 
 return None and a string with an error message to be output to the terminal.
 6. Take the value of search_resources, load the it as a json string, extract the content
  and decode it.
 7. If there are results, begin counting the results and set matching and non-matching
 results as dictionaries with URI's as keys and the resource title as their values.
 8. `for id_num in total_id_lines:` - the following is a little confusing, but the basic
 summary is we want to matchthe resource identifier with what the user input. If the 
 identifier is more than 1 part, we are checking that the splits we did above on the 
 user input match the number of id parts that exist in the json data for the resource.
 If the above is true, get the URI for the resource, split it on "/"'s and take the 
 last number as the value of `resource_uri` and the 3rd part [2] index as the 
 `resource_repo`
 9. Any results that fail this are put in the `non_match_results` dictionary and None is
 returned along with an error message detailing what resources did show up in the search.
#### export_ead()
##### Parameters
1. input_id - accepts 1 resource identifier at a time (str)
2. resource_repo - resource repository number (int)
3. resource_uri - resource URI (int). Really this is the identifier ArchivesSpace uses
4. as_username - (str)
5. as_password - (str)
6. as_api - (str)
7. include_unpublished=False - Optional parameter - need to integrate
8. include_daos=True - Optional parameter - need to integrate
9. numbered_cs=True - Optional parameter - need to integrate
10. print_pdf=False - Optional parameter - need to integrate
11. ead3=False - Optional parameter - need to integrate

This function handles exporting EAD.xml files from ArchivesSpace. The steps for this 
function are as follows:
1. Try, Except to initiate an ASnake client. ArchivesSnake is a package to help work with
the ArchivesSpace API. Assign the client as variable `client`.
2. Run a client.get (similar to request.get) to make a call to the ArchivesSpace API to
export an EAD.xml file of a specific resource record.
    1. In the ArchivesSpace API, we use the endpoint 
    repositories/{}/resource_descriptions/{}.xml? as documented here:
     https://archivesspace.github.io/archivesspace/api/#get-an-ead-representation-of-a-resource
    2. Parameters:
        1. `'include_unpublished': False` - doesn't include unpublished portions of the resource
        2. `'include_daos': True` - include digital objects
        3. `'numbered_cs': True` - include numbered container levels
        4. `'print_pdf': False` - do not export resource as pdf 
        5. `'ead3': False` - do not use EAD3 schema, instead defaults to EAD2002
3. `if request_ead.status_code == 200:` - check if request was successful
    1. `if "/" in input_id:` - some resource identifiers contain backslashes. This messes
    with assigning a filepath, so we swap / with _
4. `with open(filepath, "wb") as local_file:` - write the file's content to the 
source_eads folder and return the filepath and result string (" Done")
### cleanup.py
This script runs a series of xml and string cleanup operations on an EAD.xml file. The
function `cleanup_eads` iterates through all the files in the folder source_eads
and creates a class instance of `EADRecord`, running a host of class methods on the 
EAD.xml file and returns it to the function, which writes the new file to the folder
clean_eads.

The code begins with a for loop checking if the folders source_eads and clean_eads
exist. If either do not exist, they are created there.
#### Class EADRecord
This class hosts 12 different methods, each designed to modify an EAD.xml file according
to guidelines set by the Hargrett and Russell library for display on their XTF finding
aid websites, as well as other measures to match some of Harvard's EAD schematron
checks. This produces an EAD.xml file that is EAD2002 compliant.

The following methods will be described briefly.
1. `add_eadid()` - Takes the resource identifier as listed in ArchivesSpace and
copies it to the <eadid> element in the EAD.xml file.
2. `delete_empty_notes()` - Searches for every `<p>` element in the EAD.xml file and 
checks if there is content in the element. If not, it is deleted.
3. `edit_extents()` - Does 2 things. It deletes any empty `<extent>` elements and 
removes non-alphanumeric characters from the beginning of extent elements. An example
would be: `<extent>(13.5x2.5")</extent>`. This would change to `<extent>13.5x2.5"</extent>`.
4. `add_certainty_attr()` - Adds the attribute `certainty="approximate"` to all dates
that include words such as circa, ca. approximately, etc.
5. `add_label_attr()` - Adds the attribute `label="Mixed Materials"` to any container
element that does not already have a label attribute.
6. `delete_empty_containers()` - Searches an EAD.xml file for all container elements
and deletes any that are empty.
7. `update_barcode()` - This adds a `physloc` element to an element when a container has
a label attribute. It takes an appended barcode to the label and makes it the value
of the physloc tag.
8. `remote_at_leftovers()` - Finds any unitid element with a type that includes an 
Archivists Toolkit unique identifier. Deletes that element.
9. `count_xlinks()` - Counts every attribute that occurs in a `<dao>` element.
10. `clean_unused_ns()` - Removes any unused namespaces in the EAD.xml file.
11. `clean_do_dec()` - Removes `xlink:` prefixes in all attributes. Replaces other
namespaces not removed by `clean_unused_ns()` in the `<ead>` element with an empty
`<ead>` element.
12. `clean_suite()` - Runs the above methods according to what the user specified
in the custom_clean parameter. Will also add a doctype to the EAD.xml file and encode
it in UTF-8.
#### cleanup_eads()
This function iterates through all the files located in source_eads, uses the lxml
package to parse the file into an lxml element, passes the lxml elemnent through the
EADRecord class, runs the clean suite against the lxml element, then writes it
to a file in our clean_eads folder.
##### Parameters
1. custom_clean (list)- A list of strings as passed from as_xtf_GUI.py that determines
what methods will be run against the lxml element when running the `clean_suite()`
method. The user can specify what they want cleaned in as_xtf_GUI.py, so this
is how those specifications are passed.
2. keep_raw_exports=False - Optional Parameter - if a user in as_xtf_GUI.py specifies
to keep the exports that come from as_export.py, this parameter will prevent the
function from deleting those files in source_eads. NOT SURE is this is how I want it
to be designed.
