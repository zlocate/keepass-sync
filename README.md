# keepass-sync
Simple bash script that allows you to automatically synchronize the Keepass password database with a cloud storage. To use the script, you need to install the rclone utility that provides access to various cloud storages.
With keepass-sync, you can set up regular automatic copying of the password database to a cloud storage to ensure its safety and accessibility on multiple devices.

Original script source:  https://medium.com/@z.baratz/setting-up-keepassxc-on-linux-with-cloud-synchronization-85ccce837365

## Important note:
If you don't synchronize but then edit the other file, the newer modification time on the second file edited will cause data to be overridden. The solution would be to merge manually (Database --> Merge from database).

## Installation
1. Firstly you need to install and configure rclone. In this example, we will look at the setup for Dropbox.
a. Install rclone by following the instructions on the (official website)[https://rclone.org/install/].
b. Then run the rclone config command and follow the setup wizard instructions. Below are the settings that you need to specify when setting up for Dropbox:
```
n) New remote
name> dropbox-keepass-sync
Type of storage to configure.
Choose a number from below, or type in your own value
 [snip]
15 / Dropbox
   \ "dropbox"
Dropbox App Client Id
leave blank normally.
client_id>
Dropbox App Client Secret
leave blank normally.
client_secret>
Edit advanced config?
y) Yes
n) No
y/n> n
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes
n) No
y/n> y
```
Then, authorize yourself on the Dropbox page by entering your login and password.
Finally, check the connection by running the command:
`rclone ls dropbox-keepass-sync:`
If everything is set up correctly, you should see a list of files in the root of Dropbox.

2. Clone the repository and grant execution permissions to the script:
`git clone https://github.com/zlocate/keepass-sync.git && chmod +x ./keepass-sync/keepass-sync.sh`
3. Configure the script. Open keepass-sync.sh and replace
```
# Name of your remote storage as defined in Rclone
DRIVE_NAME="dropbox-keepass-sync"
# Name and locations of the passwords file
DB_FILE_NAME="passwords.kdbx"
LOCAL_LOCATION="/home/user/passwords/"
```
Set the `DRIVE_NAME` variable to the name of your rclone profile that you specified in step 1.

Set the `DB_FILE_NAME` variable to the name of your Keepass password database file.

Set the `LOCAL_LOCATION` variable to the path to your password database file on your local machine.

Save the changes and close the file. Now, when you run the script, it will synchronize your Keepass password database with the cloud storage you configured in rclone.

## TODO:

1. Add a systemd service that monitors changes to the password database file.
2. Use shared files in KeepassXC and import/export procedures.
3. Automatically install rclone if it's not already installed.