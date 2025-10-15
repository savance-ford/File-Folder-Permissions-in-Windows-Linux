# Creating, Modifying, and Removing File and Folder Permissions in Windows


In this lab, you'll create and change folder permissions using the Windows Command Line Interface (CLI), known as Powershell. In this exercise, you'll:

access administrative privileges to use Powershell in Windows.
view file and folder permissions using the GUI and Powershell commands.
modify the permissions for both files and directories by granting and removing specific permissions using ICACLS in Powershell.
modify the permissions for groups using the GUI and Powershell.


Example 1
In "C:\Users\Qwiklab\Documents\" you have a file named "important_document." Your goal in this example is to change the permissions so that the user "Kara" only has read access to the file.

First, use ICACLS to view the existing permissions for the file using this command:

<pre style="background: #fdfdfd; color: black;">
ICACLS C:\Users\Qwiklab\Documents\important_document
</pre>

#### OUTPUT
```
C:\Users\Qwiklab\Documents\important_document QWIKLABS-BB-5A8\Kara:(R,W)
                                              NT AUTHORITY\SYSTEM:(I)(F)
                                              BUILTIN\Administrators:(I)(F)
                                              BUILTIN\Users:(I)(RX)
                                              Everyone:(I)(RX)

Successfully processed 1 files; Failed processing 0 files

```

As you can see, Kara currently has read and write permissions (shown as "R" and "W"). We need her to only have read permissions, so we need to remove her write permission. An easy way to accomplish this is to remove all of Kara's permissions and then re-add her read permission. You can remove her entirely from the file's permissions and check to see that it worked with these commands:

```
ICACLS C:\Users\Qwiklab\Documents\important_document /remove "Kara"
```

```
processed file: C:\Users\Qwiklab\Documents\important_document
Successfully processed 1 files; Failed processing 0 files
```

```
ICACLS C:\Users\Qwiklab\Documents\important_document
```

<pre  style="background-color: black; color: white; padding: 1em;">
```

C:\Users\Qwiklab\Documents\important_document NT AUTHORITY\SYSTEM:(I)(F)
                                              BUILTIN\Administrators:(I)(F)
                                              BUILTIN\Users:(I)(RX)
                                              Everyone:(I)(RX)

Successfully processed 1 files; Failed processing 0 files

````
</pre>

As you can see, Kara is no longer listed in the file's permissions. To re-grant her only the read permission, use this command:

```
ICACLS C:\Users\Qwiklab\Documents\important_document /grant "Kara:(r)"
```

```
processed file: C:\Users\Qwiklab\Documents\important_document
Successfully processed 1 files; Failed processing 0 files
```

Now the file's permissions should be set correctly, with Kara only having read permissions. You can double check this with the earlier command:

```
ICACLS C:\Users\Qwiklab\Documents\important_document
```

```
C:\Users\Qwiklab\Documents\important_document QWIKLABS-BB-066\Kara:(R)
                                              NT AUTHORITY\SYSTEM:(I)(F)
                                              BUILTIN\Administrators:(I)(F)
                                              BUILTIN\Users:(I)(RX)
                                              Everyone:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```

## Example 2

There's a folder called "Secret" in "C:\Users\Qwiklab\" where the user "Kara" has read access. Your goal in this example is to alter these permissions so that another user (named "Phoebe") has read permissions as well, and Kara has both read and write permissions. You can view the current permissions with this command, and see that Kara has read permissions and Phoebe is not included.

```
ICACLS C:\Users\Qwiklab\Secret\
```

```
C:\Users\Qwiklab\Secret\ QWIKLABS-BB-066\Kara:(R)
                         NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
                         BUILTIN\Administrators:(I)(OI)(CI)(F)
                         BUILTIN\Users:(I)(RX)
                         BUILTIN\Users:(I)(OI)(CI)(IO)(GR,GE)
                         Everyone:(I)(RX)
                         Everyone:(I)(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

First, give Phoebe read access. You can grant her these permissions with the command below, like before. Then, use the previous command again to verify that the change has been made:


```
ICACLS C:\Users\Qwiklab\Secret\ /grant "Phoebe:(r)"
```

```
processed file: C:\Users\Qwiklab\Secret\
Successfully processed 1 files; Failed processing 0 files
```

```
ICACLS C:\Users\Qwiklab\Secret\
```

```
C:\Users\Qwiklab\Secret\ QWIKLABS-BB-066\Phoebe:(R)
                         QWIKLABS-BB-066\Kara:(R)
                         NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
                         BUILTIN\Administrators:(I)(OI)(CI)(F)
                         BUILTIN\Users:(I)(RX)
                         BUILTIN\Users:(I)(OI)(CI)(IO)(GR,GE)
                         Everyone:(I)(RX)
                         Everyone:(I)(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

The next step is to grant Kara write permissions. You don't need to remove her existing permissions first, like you did before; you only need to add "write" to her existing permissions with this command:

```
ICACLS C:\Users\Qwiklab\Secret\ /grant "Kara:(w)"
```

```
processed file: C:\Users\Qwiklab\Secret\
Successfully processed 1 files; Failed processing 0 files
```

Now the file should have the required permissions. View them to verify this with the following command:

```
ICACLS C:\Users\Qwiklab\Secret\
```

```
C:\Users\Qwiklab\Secret\ QWIKLABS-BB-066\Phoebe:(R)
                         QWIKLABS-BB-066\Kara:(R,W)
                         NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
                         BUILTIN\Administrators:(I)(OI)(CI)(F)
                         BUILTIN\Users:(I)(RX)
                         BUILTIN\Users:(I)(OI)(CI)(IO)(GR,GE)
                         Everyone:(I)(RX)
                         Everyone:(I)(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

## Example 3

There's another folder in "C:\Users\Qwiklab\" called "Music". The user group, named "Everyone", has both read and write permissions for this folder. User groups are sets of local users that allow you to change multiple users' permissions at once. For example, a computer that's used by lots of employees at a business may have a usergroup called "Employees" that new hires are added to when they onboard. This automatically gives them access to everything they need, without it having to be set manually. The "Everyone" group is created by default, and every new user is automatically added.

Your goal for this example is to change the permissions for this folder so that the "Everyone" group only has read permission (not write).

As usual, view the current permissions with this command:

```
ICACLS C:\Users\Qwiklab\Music\
```

```
C:\Users\Qwiklab\Music\ Everyone:(R,W)
                        NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
                        BUILTIN\Administrators:(I)(OI)(CI)(F)
                        BUILTIN\Users:(I)(RX)
                        BUILTIN\Users:(I)(OI)(CI)(IO)(GR,GE)
                        Everyone:(I)(RX)
                        Everyone:(I)(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

You can change permissions for groups in exactly the same way as you do for users. To remove the group's current permissions, and then re-grant them a read permission, use these commands:

```
ICACLS C:\Users\Qwiklab\Music\ /remove "Everyone"
```

```
processed file: C:\Users\Qwiklab\Music\
Successfully processed 1 files; Failed processing 0 files

```

```
ICACLS C:\Users\Qwiklab\Music\ /grant "Everyone:(r)"
```

```
processed file: C:\Users\Qwiklab\Music\
Successfully processed 1 files; Failed processing 0 files
```

The "Everyone" group should now have only read permissions, which you can check using the same command as before:

```
ICACLS C:\Users\Qwiklab\Music\
```

```
C:\Users\Qwiklab\Music\ Everyone:(R)
                        NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
                        BUILTIN\Administrators:(I)(OI)(CI)(F)
                        BUILTIN\Users:(I)(RX)
                        BUILTIN\Users:(I)(OI)(CI)(IO)(GR,GE)
                        Everyone:(I)(RX)
                        Everyone:(I)(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

## Example 4

Back in the "documents" folder from before, there's a file called "not_so_important_document". In this example, you need to modify the permissions for that file so that the group called "Authenticated Users" has "Write" access. The "Authenticated Users" group contains users who have authenticated to the domain or a domain that is trusted by the computer. View the current permissions with this command, to see what the starting point for this file is:


```
ICACLS C:\Users\Qwiklab\Documents\not_so_important_document
```

```
C:\Users\Qwiklab\Documents\not_so_important_document 
NT AUTHORITY\SYSTEM:(I)(F)
BUILTIN\Administrators:(I)(F)
BUILTIN\Users:(I)(RX)
Everyone:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```
This will show you that the "Authenticated Users" group is currently not listed. This means that the only step required is to grant them write access, which you can do with this command:

```
ICACLS C:\Users\Qwiklab\Documents\not_so_important_document /grant "Authenticated Users:(w)"
```

```
processed file: C:\Users\Qwiklab\Documents\not_so_important_document
Successfully processed 1 files; Failed processing 0 files
```

That should successfully grant them write permissions. You can use the same command as earlier to verify that the commands were a success:

```
ICACLS C:\Users\Qwiklab\Documents\not_so_important_document
```

```
C:\Users\Qwiklab\Documents\not_so_important_document 
NT AUTHORITY\Authenticated Users:(W)
NT AUTHORITY\SYSTEM:(I)(F)
BUILTIN\Administrators:(I)(F)
BUILTIN\Users:(I)(RX)
Everyone:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```

## Example 5

In this final example, you'll change the permissions of another file in the "Documents" folder. The file named "public_document" needs to be made publically readable, so that anyone on the system is able to read it. As usual, view the file's commands first:

```
ICACLS C:\Users\Qwiklab\Documents\public_document
```
```
C:\Users\Qwiklab\Documents\public_document NT AUTHORITY\SYSTEM:(I)(F)
                                           BUILTIN\Administrators:(I)(F)
                                           BUILTIN\Users:(I)(RX)
                                           Everyone:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```


The easiest way to make sure that all users on the system have read permissions is to add those permissions to the "Everyone" group. You could also add each user manually, but by giving the group the permissions instead, you save time; it removes the need to go back and edit permissions again if a new user is created in the future. Grant every user on the system the ability to read the file using this command:

```
ICACLS C:\Users\Qwiklab\Documents\public_document /grant "Everyone:(r)"
```

```
processed file: C:\Users\Qwiklab\Documents\public_document
Successfully processed 1 files; Failed processing 0 files
```

Finally, view the permissions again to make sure it worked:

```
ICACLS C:\Users\Qwiklab\Documents\public_document
```

```
C:\Users\Qwiklab\Documents\public_document Everyone:(R)
                                           NT AUTHORITY\SYSTEM:(I)(F)
                                           BUILTIN\Administrators:(I)(F)
                                           BUILTIN\Users:(I)(RX)
                                           Everyone:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```

### Conclusion

Wohoo! You've successfully used Powershell to modify the permissions for both files and directories. You modified the permissions by granting and removing specific permissions using ICACLS. You've also become familiar with groups of users, and how to modify permissions for them as well. Really well done.