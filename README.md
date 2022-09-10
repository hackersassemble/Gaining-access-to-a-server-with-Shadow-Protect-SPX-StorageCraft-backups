# Gaining-access-to-a-server-with-Shadow-Protect-SPX-StorageCraft-backups

==
1. RDP access to one of the servers, in my case it is a SQL server.
2. On the desktop, we see the ShadowProtect SPX icon
   -> click on it.
3. the GUI opens (if you are prompted for credentials, enter the credentials under which you signed in via RDP, or any other software)
4. On the left side in the "Job Summary" block you will see the detailed description of backup scheme
   In the "Name" field - backup name of our server
   in the "Destination" field - place WHERE our spx stores backup copies, as BACKUP NAME (BALL WITH BACKUP ON THIS SERVICE)
      From our example we can conclude that all backup files are stored in a ball named StorageCraft, and the folders with backup servers are named with the name of the server itself.
5. Knowing the name of backup server, we want to get more information about his structure, the first thing we do is to get a balloon with the command "cmd.exe> net view \\\COH-DSS3 /ALL", in response we get "Error 5: Access Denied".
6. No access, trying to bang on the accounts of other people - the answer is the same - Error n 5, it would be logical to assume that in order to gain access to the server, we need either the credentials of the local admin on this very server, or account of a special user with special privileges
7. Let's assume that if it is a dedicated user, he has a similar name to the software/function:
   we go through the logins with substrings (here we need to get fancy):
      Storage
      Shadow
      Protect
      Craft
      SP
      SPX
      Backup
      BUUser
      ETC.
   Then do a search for ntds.dit (hashes.txt.ntds) to find the hash, in my case, the search was successful and I found the user Humanity.local\SPAdmin (I think it is clear that it is Shadow Protect Admin) and its hash ce31b806821bec116ba03132ab5b3138, but unfortunately, search on cmd5.org not result and I need the urgent clearance. (If you have enough hash, congratulations - you got the result.)
8. But if you still need a clipart or you can not find the right user, we understand that if the software somehow knocked on the server, she knows the credentials, which means they can stay on the server.
   Try to dump hashes
   Here I will not describe in detail how to do it, but you should try hashdump (and its legitimate analogues) and logonpasswords (and similar)
   In my case I used mimic and saved the passwords and found the clirapass from my SPAdmin account - kerberos:
         * Username : SPAdmin
         * Domain : COHBackup
         * Password : Backup!User
    (in my case for some reason the domain was not Humanity.local but COHBackup, although you can also knock with Humanity.local (replace it with your own value))
9. Going into Explorer, and open through it the necessary sphere "\\COH-DSS3\StorageCraft" at me asks for credentials, I enter COHBackup\SPAdmin and Backup!User and successfully get access
10. Also in some networks backup servers can be a few, as an option to check this, is to click on the button Backup in the upper left corner of the gui (just after File) then Destinations -> and we will see what is the way to save the backup
===

CREDITS: Learners Room @dtob1804
