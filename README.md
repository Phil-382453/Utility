1. Depending where you unzipped the download zip file... you will need to modify the main script as shown below
   The unzip will create a folder name "ScriptWrapper". Edit this file...\ScriptWrapper\ScriptWrapper.ps1
   At line 30 (approx) edit this and point it to the folder where you want this tool to reside. 
     $SourceFolder = "F:\PowerShell\ScriptWrapper" - This can be a local or UNC path.
2. In come cases your lalptop gives an error about running scripts. Here is the solution.
   Launch PowerShell as Admin to make this change.
   Change the PowerShell execution policy - run this command.
     set-ExecutionPolicy RemoteSigned -Scope LocalMachine
   Exit PowerShell before opening PowerShell again to launch the script. 
3. Launch the script with PowerShell (or PowerShell_ISE) as a user. Do not use RunAs.
   This script must be launched as a regular user to have access to your profile path.
   The ScriptsWrapper will prompt for privileged credentials when accessing a remote server(s).
4. In a team environment, the ScriptWrapper Source files (Unzip) should be stored on a UNC path.
       Team members launch the main file (ScriptWrapper.ps1) from the UNC path location.
   The ScriptsWrapper (on 1st launch) uses your local profile path to create a cache folder.
   Cache files include the following:
   - Subfolder (Config)          - Settings for Creds, Operation (local/remote), etc.
   - Subfolder (Modules)         - Custom PowerShell modules required by the main app.
   - Subfolder (ScriptsTemplate) - files used when creating or adding new scripts.
   - Subfolder for each script repository. Output files are saved here.
   - Delete this local cache folder for a complete removal of the ScriptsWrapper tool.
5. Purpose:
   This tool was created to address the difficulties that come with an ever growing PowerShell Scripts repository.
   This tool handles many of the tasks that a standalone script would do, such as:
    - Reading in the Server List
    - Invoking the (selected) script on the remote server(s) 
    - Collecting the remote results (when complete)
    - Displaying the results
    - Saving output to excel or notepad
    - Logging script usage
--------------------------
* The Main app window:
   - Top    - Serveral menu items.
   - Top    - Title with an expander for basic settings.
   - Top    - Row of buttons including where you select the scripts folder path.
   - Left   - Textbox for adding computer names.
   - Right  - Tab window (Red Main tab) + results tabs as you run scripts.
1. Title with expander for basic settings.
   - Credentials
     Manual Creds - Credentials are encrypted and stored in a variable.
     Creds Manager - Uses the control panel credentials manager to securely store credentials for various domains.
   - Run Scripts on
     Remote Servers - Use this to send scripts to remote servers for execution on your list of servers
     Local Only - Use this to force all scripts to only run on the local machine.
   - Add/Remove Script Folders - Add a new script repository (folder) to this tool.
     The new folder can be a UNC path or local folder.
2. Select a scripts folder (repository) ... and click the refresh button.
   The ScriptWrapper will read all the .ps1 files in the folder and build a list of script for display on the red Main tab.
(needs cleaned up)
Select a scripts folder (repository) ... and click the refresh button:
    The ScriptsWrapper will read all the .ps1 files and build a list of scripts for display on the red Main tab.
    =============
    The red Main tab:
    The 1st column (ScriptName) is the file name.
    The 2nd column checkbox is to mark favorite scripts.
    The 3rd column - Description - comes from data inside each script file.
=================
Select and launch a script:
    Enter your list of computer names in the Servers text box (one per line).
    Select/highlight the ScriptName you want to launch.
    Click the red "Run Script" button ... double-click a script name will also launch it.
    You will be prompted for credentials if needed.
=================
Main (red) Tab:
    Aways the 1st tab to open, it contains the following: 
    1. The list of Scripts - This includes the Script Name, Favorites checkbox, and script Description.
       Script Name - Red   - The script has the ability to modify a server(s).
       Script Name - Black - Collect info only ... no modifications to server(s).
       Description - Blue  - The script has an InputTab.
       Description - Green - Special access scripts. You will probably not see any of these.
       The favorites checkbox - checked items are displayed 1st, at the top of the list.

    2. "Filter Scripts on Keyword" Textbox - This allows you to find matching scripts quickly. 
       Keywords fall into 3 categories:
       Function - Keywords = Patches, Network, Drives, CPU, Firmware, etc.
       Script   - Keywords = Standalone, Text, Table, InputTab
       Action   - Keywords = Warning, AdminScript, NewScript
       
       Scratchpad (Keyword) - is a unique script type. It allows you to enter your script-block in a textbox.
       The script-block you enter is then sent to your server list for remote processing.
=================
Other Tabs:
    Output Tab - script results are sent to a new output tab every time a script returns output.
    Errors Tab - Errors are sent to an Error Tab (and file).
	       - The Errors file is overwritten each time you launch a script. 
    Input Tab  - Some scripts need you to enter input and make some selections.
    Help Tab   - Seems you found this one :)

    Output is sent to its own tab. You can have text or table output.
    Output tabs - have a row of buttons at the bottom. 
      Send output to notepad - available for text  output.
      Send output to excel   - available for table output.
      The Close button       - will close the current tab.
      The Close All button   - will close all tabs (except for the Main Tab).
===============================================================================================
Developers Note: 
    Template Scripts are used when creating your own custom scripts. They provide a guide to follow. 
    Create and run your new scripts in (settings) local only mode until you are sure they work correctly.
    Choose the template type of script you want to create.
    Copy and Rename the template script to a new scripts folder. 
    Edit the template script as needed. Save your changes.
    Click the Refresh button (path to your new script) - this will pull script updates into this tool.
    Repeat as needed ... Edit script, Save, Refresh button, Run (verify).

    The top of each script contains a help section that is required for your script to work in this tool.
    .Synopsis      - Provide a description of the script (Main tab - 3rd column of the scripts list).
    .Functionality - a comma separated list of keywords. (used with Find Scripts by Keyword(s)).
    .Output        - Text, Table, MultiSheet, InputTab, Local, or Standalone.

    Text       - Output/Results are sent to a text based output tab. Bottom buttons let you send this to notepad.
    Table      - Output/Results are sent to a table based output tab. Bottom buttons let you send this to excel.
    Multisheet - Output/Results are sent to a multiple output tabs. Each tab can be text or table based on your choices.
    InputTab   - Used when your script has user choices prior to launching it on a remote server list.
    Standalone - Used to quickly move an existing PowerShell script into the ScriptsWrapper tool.
               - You add (copy and paste) a help section to the top of your script. Add the PAUSE cmd to the end.
                 Then drop it in a 'Scripts' folder. Click the refresh button and it is immediately available.
                 The script launches in a separate PowerShell instance, which closes immediately upon completion. 
                 None of the advanced features are available. The only benefit - launching your script from inside the tool.
    local      - This will force the script to run on the local machine only, regardless of the choice under settings. 
                 Sometimes you have a script that needs to run locally (always). Example - sending data to Outlook email.
=================

