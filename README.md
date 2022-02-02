CryptoBlocker
==============

This is a solution to block users infected with different ransomware variants.

The script will install File Server Resource Manager (FSRM), and set up the relevant configuration.

<b>Script Deployment Steps</b>

<i><b>NOTE:</b> Before running, please add any known good file extensions used in your environment to <tt>SkipList.txt</tt>, one per line.  This will ensure that if a filescreen is added to the list in the future that blocks that specific file extension, your environment won't be affected as they will be automatically removed.  If SkipList.txt does not exist, it will be created automatically.</i>

1. Checks for network shares
2. Installs FSRM
3. Create batch/PowerShell scripts used by FSRM
4. Creates a File Group in FSRM containing malicious extensions and filenames (pulled from https://fsrm.experiant.ca/api/v1/get)
5. Creates a File Screen in FSRM utilising this File Group, with an event notification and command notification
6. Creates File Screens utilising this template for each drive containing network shares

<b> How it Works</b>

If the user attempts to write a malicious file (as described in the filescreen) to a protected network share, FSRM will prevent the file from being written and send an email to the configured administrators notifying them of the user and file location where the attempted file write occured.

<b>NOTE: This will NOT stop variants which use randomised file extensions, don't drop README files, etc</b>

<b>Usage</b>

Just run the script.  You can easily use this script to deploy the required FSRM install, configuration and needed blocking scripts across many file servers

An event will be logged by FSRM to the Event Viewer (Source = SRMSVC, Event ID = 8215), showing who tried to write a malicious file and where they tried to write it. Use your monitoring system of choice to raise alarms, tickets, etc for this event and respond accordingly.

<b>ProtectList.txt</b>

By default, this script will enumarate all the shares running on the server and add protections for them. If you would like to override this, you can create a <tt>ProtectList.txt</tt> file in the script's running directory. The contents of this file should be the folders you would like to protect, one per line. If this file exists, only the folders listed in it will be protected. If the file is empty or only has invalid entries, there will be no protected folders.

<b>IncludeList.txt</b>

Sometimes you have file screens that you want to add that are not included in the download from Experiant. In this case, you can simply create a file named <tt>IncludeList.txt</tt> and put the screens you would like to add, one per line. If this file does not exist, only the screens from Experiant are included.

<b>Disclaimer</b>

This script is provided as is.  I can not be held liable if this does not thwart a ransomware infection, causes your server to spontaneously combust, results in job loss, etc.
