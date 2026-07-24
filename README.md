* Purpose of this tool
   - This tool was created to address the difficulties that come with an ever growing PowerShell Scripts repository.
   - Benefits include the ability to manage thousands of scripts in hundreds of repositories.
   - The only requirement for adding a new script is 3 lines of "comment-based help" at the script top. See Developers Note below.
   - This tool handles many of the tasks that a standalone script would do, such as:
     - Reading in the Server List
     - Invoking the (selected) script on the remote server(s) 
     - Collecting the remote results (when complete)
     - Displaying the results
     - Saving output to excel or notepad
     - Logging script usage

* Download the Zip file. Unzip will create a folder (ScriptWrapper).
  - Copy this folder to its final location.
  - Inside this folder is the main application file (ScriptWrapper.ps1)...a powershell script.
  - Reading this script in PowerShell_ISE is helpful to low or mid level programmers. It is extensively documented.
  - It is easiest to read this script using ctrl+m to collapse all the expandable sections. Then expand as needed.

* Install (and future app launches) consists of Running this file (ScriptWrapper.ps1) using the default user account.
  - In some cases, your laptop might give an error about running scripts. The most common solution follows.
     - Launch PowerShell as Admin. Change the PowerShell execution policy to remote signed - run this command.
     - set-ExecutionPolicy RemoteSigned -Scope LocalMachine
  - This will copy a subfolder (New_Cache) from the (source) zip files
    to a new (ScriptWrapper) folder on the local machine, in the users local profile. This is called the users/local cache.
  - Everytime you launch this script, robocopy will compare the cache files, last modified date property,
    to the source file last modified date. If a difference is found, robocopy will re-sync the modified files on the client. 
  - launch continues to load the rest of the script including the application window.
  - If you are reading the code in PowerShell_ISE ... code to draw the app window is at the bottom of the script.
  - Once the app window loads, the user can select the desired scripts repo and click the refresh button.
  - The refresh button will populate the red Main tab with the script in the selected repository.
  - The (sync) during launch prevents the client from modifying the default files between application launches.
  - This also allows the source files to be updated then automatically synchronized to the client every launch.
  - In a team environment, the ScriptWrapper Source files (unzip) should be stored on a shared UNC path.
     - Team members launch the main file (ScriptWrapper.ps1) from the UNC path location.
     - I use a batch file on the desktop ... start PowerShell_ISE.exe "F:\PowerShell\ScriptWrapper\ScriptWrapper.ps1"
* Cache files copied (New_Cache) include the following:
   - Subfolder (Config)          - Initial settings for Creds, Operation (local/remote), etc.
   - Subfolder (Modules)         - Custom PowerShell modules required by the main app.
   - Subfolder (ScriptsTemplate) - files used when creating or adding new scripts.
   - Subfolder - for each script repository listed on the Ren Main tab. Output files are saved here.
   - Source files not copied include a folder (script repo) called DevScripts that includes 44 working powershell scripts.

* Uninstall - Uninstall button on the main app window under settings...This will delete the wrapper local cache files.
   You can also delete this local cache folder to uninstall the client files.
  
* The Main app window: See the "Screenshot Main App.jpg" file
   - Top    - Serveral menu items (Help,Folders,Exit)
   - Top    - Settings - This is an expander to access the settings.
     - a. Simple Admin credentials vs Control Panel Credentials Manager
     - b. Run scripts on local machine vs Push scripts out to remote servers.
     - c. Uninstall - deletes the local cache folder
     - d. Add/Remove script repository folder paths
   - Top    - Row of buttons including a drop down list, where you select the repository folder path.
   - Left   - Textbox for adding computer names (add 1 name or add 10,000 names).
   - Bottom - Tab window (Red Main tab) + results tabs. New result tab opens every time you run a script.

* Red Main Tab 
   - Select a script repository then click the green "Refresh" button (Top right).
   - The red Main tab fills with the list of scripts in the selected repository.
   - The 1st column - ScriptName - this is the filename in the repository folder.
   - The 2nd column - checkbox to mark favorite scripts. Favorite scripts are sorted at the top after refresh.
   - The 3rd column - Description - comes from a variable in the script file "context-sensitive help"

* Launching a script. 
   - Enter you list of servers in the left side text box.
   - Select a script to run. Hi-lite and click the run button - or dbl-click script name.
   - For remote execution mode, you will be prompted for credentials to the server(s).
   - The Main app window will minimize and return once the script run is complete.
   - Script results are written to file and sent to an output tab. See the "Screenshot Output.jpg" file
   - All tabs except the main tab can be closed at any time.

* Main (red) Tab Items
    Aways the 1st tab to open (cannot be closed), it contains the following: 
    1. The list of Scripts - This includes the Script Name, Favorites checkbox, and script Description.
       - Script Name - Red   - The script has the ability to modify a server(s).
       - Script Name - Black - Collect info only ... no modifications to server(s).
       - Description - Blue  - The script has an InputTab.
       - The favorites checkbox - checked items are displayed at the top of the list of scripts.

    2. "Filter Scripts on Keyword" Textbox - This allows you to find matching scripts quickly. 
       - Keywords fall into 3 categories:
       - Function - Keywords = Patches, Network, Drives, CPU, Firmware, etc.
       - Script   - Keywords = Standalone, Text, Table, InputTab
       - Action   - Keywords = Warning, AdminScript, NewScript

    3. Other Tabs
       - Output Tab - script results are sent to a new output tab every time a script returns output.
       - Errors Tab - Errors are sent to an Error Tab (and file).
	     The Errors file is overwritten each time you launch a script. 
       - Input Tab  - Some scripts need you to enter input and make some selections (vs running on the server immediately).
	     These scripts contain the code to draw the input tab along with the code to run on the server.

	4. Output is sent to its own new tab. You can have text or table output.
       - Output tabs - have a row of buttons at the bottom. 
         - Send output to notepad - available for text  output.
         - Send output to excel   - available for table output.
         - The Close button       - will close the current tab.
---------------------------
* Developers Note: 
    - Template Scripts (default repository) are used when creating your own custom scripts. They provide a guide to follow. 
    - Create and run your new scripts in (settings) local only mode until you are sure they work correctly.
    - Choose the template type of script you want to create.
    - Copy and Rename the template script to a new scripts folder. 
    - Edit the template script as needed. Save your changes.
    - Click the Refresh button (path to your new script) - this will pull script updates into this tool.
    - Repeat as needed ... Edit script, Save, Refresh button, Run (verify).
	
    - The top of each script contains a help section that is required for your script to work in this tool:
    -      .Synopsis      - Provide a description of the script (Main tab - 3rd column of the scripts list).
    -      .Functionality - a comma separated list of keywords. (used with Find Scripts by Keyword(s)).
    -      .Output        - Text, Table, MultiSheet, InputTab, Local, or Standalone.
      -     Text  - Output/Results are sent to a text based output tab. Bottom buttons let you send this to notepad.
      -     Table - Output/Results are sent to a table based output tab. Bottom buttons let you send this to excel.
      -     Multisheet - Results are sent to multiple output tabs. Each tab can be text or table based on your choices.
      -     InputTab   - Used when your script has user choices prior to launching it on a remote server list.
      -     Standalone - Used to quickly move an existing PowerShell script into the ScriptsWrapper tool.
      -         You add (copy and paste) a help section to the top of your script. Add the PAUSE cmd to the end.
      -         Then drop it in a 'Scripts' folder. Click the refresh button and it is immediately available.
      -         The script launches in a separate PowerShell instance, which closes immediately upon completion. 
      -         None of the advanced features are available. The only benefit - launching your script from inside the tool.
      -     local - This will force the script to run on the local machine only, regardless of the choice under settings. 
      -         Sometimes you have a script that needs to run locally (always). Example - sending data to Outlook email.
