
* Download the Zip file. Unzip will create a folder (ScriptWrapper). Copy this folder to its final location.
Inside this folder is the main application file (ScriptWrapper.ps1)...a powershell script.
Reading this script in PowerShell_ISE is helpful to low or mid level programmers. It is extensively documented.

* Install consists of Running this file (ScriptWrapper.ps1):
    This will copy a subfolder (New_Cache) from 
    the (source) zip files to a new (ScriptWrapper) folder on the local machine, in the users local profile.
  Every time you launch this script, it will sync the New_Cache to the source if the source is different from the cache. 
   Then it continues to load the rest of the script including the application window.
   This (sync) prevents the client from modifying the default files between application launches.
   This also allows the source files to be modified then automatically synchronized to the client every launch.
1. In some cases, your laptop might give an error about running scripts. This might be the solution.
   Launch PowerShell as Admin. Change the PowerShell execution policy to remote signed - run this command.
     set-ExecutionPolicy RemoteSigned -Scope LocalMachine
2. Launch this script with PowerShell (or PowerShell_ISE) as current user. Do not use RunAs.
   This script must be launched as a regular user to have access to your profile path.
   The ScriptsWrapper will prompt for privileged credentials when accessing a remote server(s).
3. In a team environment, the ScriptWrapper Source files (unzip) should be stored on a UNC path.
       Team members launch the main file (ScriptWrapper.ps1) from the UNC path location.
       I use a batch file on the desktop ... start PowerShell_ISE.exe "F:\PowerShell\ScriptWrapper\ScriptWrapper.ps1"
   The ScriptsWrapper (on 1st launch) uses your local profile path to create a cache folder.
       Install actions are written to the powershell console.
   Cache files copied include the following:
   - Subfolder (Config)          - Settings for Creds, Operation (local/remote), etc.
   - Subfolder (Modules)         - Custom PowerShell modules required by the main app.
   - Subfolder (ScriptsTemplate) - files used when creating or adding new scripts.
   - Subfolder - for each script repository listed on the Ren Main tab. Output files are saved here.
   - The cache files copied are a subfolder of the source files.
   - Source files not copied include a folder (script repo) called DevScripts that includes 44 working powershell scripts.

* Uninstall - Uninstall button on the main app window under settings...This will delete the wrapper local cache files.
   You can also delete this local cache folder to uninstall the client files.
  
* Purpose of this tool
   This tool was created to address the difficulties that come with an ever growing PowerShell Scripts repository.
   Benefits include the ability to manage thousands of scripts in hundreds of repositories.
   The only requirement for adding a new script is 3 lines of "comment-based help" at the script top. See Developers Note below.
   This tool handles many of the tasks that a standalone script would do, such as:
    - Reading in the Server List
    - Invoking the (selected) script on the remote server(s) 
    - Collecting the remote results (when complete)
    - Displaying the results
    - Saving output to excel or notepad
    - Logging script usage

* The Main app window:
   - Top    - Serveral menu items (Help,Folders,Exit)
   - Top    - Settings - This is an expander to access the settings.
              a. Simple Admin credentials vs Control Panel Credentials Manager
              b. Run scripts on local machine vs Push scripts out to remote servers.
              c. Uninstall - deletes the local cache folder
              d. Add/Remove script repository folder paths
   - Top    - Row of buttons including a drop down list, where you select the repository folder path.
   - Left   - Textbox for adding computer names (add 1 name or add 10,000 names).
   - Bottom - Tab window (Red Main tab) + results tabs. New result tab opens every time you run a script.

* Red Main Tab (occupies 90 percent of the app window)
    Select a script repository click the refresh button (Top right). This will populate the Main tab.
    The red Main tab fills with the list of scripts in the selected repository.
    The 1st column - ScriptName - this is the filename in the repository folder.
    The 2nd column - checkbox to mark favorite scripts. 
    The 3rd column - Description - comes from a variable in the script file "context-sensitive help"

* Launching a script
    Enter you list of servers in the left side text box.
	Select a script to run. Hi-lite and click the run button - or dbl-click script name.
    The Main app window will minimize and return once the script run is complete.
    Script results will be written to file then sent to a new output tab.
    All tabs except the main tab can be closed at any time.

Main (red) Tab Items:
    Aways the 1st tab to open (cannot be closed), it contains the following: 
    1. The list of Scripts - This includes the Script Name, Favorites checkbox, and script Description.
       Script Name - Red   - The script has the ability to modify a server(s).
       Script Name - Black - Collect info only ... no modifications to server(s).
       Description - Blue  - The script has an InputTab.
       The favorites checkbox - checked items are displayed at the top of the list of scripts.

    2. "Filter Scripts on Keyword" Textbox - This allows you to find matching scripts quickly. 
       Keywords fall into 3 categories:
       Function - Keywords = Patches, Network, Drives, CPU, Firmware, etc.
       Script   - Keywords = Standalone, Text, Table, InputTab
       Action   - Keywords = Warning, AdminScript, NewScript

=================
Other Tabs:
    Output Tab - script results are sent to a new output tab every time a script returns output.
    Errors Tab - Errors are sent to an Error Tab (and file).
	       - The Errors file is overwritten each time you launch a script. 
    Input Tab  - Some scripts need you to enter input and make some selections (vs running on the server immediately).
	             These script contain the code to draw the input tab along with the code to run on the server.
    Help Tab   - Very similar to this readme file

    Output is sent to its own new tab. You can have text or table output.
    Output tabs - have a row of buttons at the bottom. 
      Send output to notepad - available for text  output.
      Send output to excel   - available for table output.
      The Close button       - will close the current tab.
      The Close All button   - will close all tabs (except for the Main Tab).

Developers Note: 
    Template Scripts (default repository) are used when creating your own custom scripts. They provide a guide to follow. 
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
