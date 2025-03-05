# ADT Eclipse Setup
Download Eclipse for Java via https://www.eclipse.org/downloads/packages/release/2024-12/r

## Install ABAP Plugins 
- Navigate to Install New -> Work with Fill -> https://tools.hana.ondemand.com/latest
- Next->Select ABAP Developement Tools-> Select All in Trusted-> Finish -> Reboot Eclipse
  
<!--
- **Window User**s: Open Eclipse-> Settings-> Search link Handlers-> choose "eclipse+command"

### Additional steps for Mac users to open ADT via Link Handlers( eclipse+command:// and adt://):
- Move Eclipse after installation to Applications folder.
- Right Click on Eclipse.app and select "**Show package contents**"
- Navigate Contents-> Info.plist
- Edit the file and insert below snippet under ```<key>CFBundleDisplayName</key> <string>Eclipse</string>``` : 
    
        <key>CFBundleURLTypes</key>
        <array>
            <dict>
                <key>CFBundleURLName</key>
                    <string>Eclipse command</string>
                <key>CFBundleURLSchemes</key>
                    <array>
                        <string>eclipse+command</string>
                    </array>
            </dict>
            <dict>
                <key>CFBundleURLName</key>
                    <string>Abap Development Tools command</string>
                <key>CFBundleURLSchemes</key>
                    <array>
                        <string>adt</string>
                    </array>
            </dict>
        </array>

- Reboot Machine
- Execute below command after reboot (Open Terminal from the Parent Folder of the app)
    **"codesign --force --deep --sign - Eclipse.app"**
- Open Eclipse-> Settings-> Search link Handlers-> choose "eclipse+command" and "adt"
   ![image](https://github.com/user-attachments/assets/9329fc69-06b3-4d35-ba51-7a1cff5b4923)
-->

## Set up Steampunk/Embedded Steampunk System:
- File-> New Abap Cloud Project-> Fill in the service instance url (Host url configured in the Destination for the respective system)
- Click on Open Logon Browser-> Validate access to system url in browser-> return to ADT
- Requested system should be open to access now.




Java Runtime	JRE version 21 (64-Bit, LTS) (*)
For Windows: SAP GUI for Windows 7.60 or higher
Microsoft VC++ Runtime  For Windows: [Microsoft Visual C++ 2015-2022 Redistributable (x64)](https://learn.microsoft.com/en-US/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2015-2017-2019-and-2022)

Get an installation of Eclipse 2024-12 (e.g. Eclipse IDE for Java Developers)

Install ABAP Development Tools (ADT) Plugin


Please refer to :

https://developers.sap.com/tutorials/abap-install-adt.html

