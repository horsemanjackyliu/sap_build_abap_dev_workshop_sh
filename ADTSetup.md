# ADT Eclipse Setup
Download Eclipse for Java via https://www.eclipse.org/downloads/packages/release/2024-09/m3

## Install ABAP Plugins 
- Navigate to Install New -> Work with Fill -> https://adt.only.sap/updatesite/dev/unbundled/
- Next-> Select All in Trusted-> Finish -> Reboot Eclipse
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


## Set up Steampunk/Embedded Steampunk System:
- File-> New Abap Cloud Project-> Fill in the service instance url (Host url configured in the Destination for the respective system)
- Click on Open Logon Browser-> Validate access to system url in browser-> return to ADT
- Requested system should be open to access now.
