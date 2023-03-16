# Connectin to SFTP via Cmd line

Instantiating an SFTP Connection with a Remote Host

When you are at the command line, the command used to start an SFTP connection with a remote host is:

    sftp username@hostname

For example, a user with the username user connecting to the remote host ada would use the following command: 

    sftp user@ada.cs.pdx.edu 

SFTP will then ask for the password to the account you’re trying to log into. After inputting it, the SFTP connection will be instantiated. An SFTP prompt will appear, looking like the following:

    sftp> 

From here, you can use a few basic linux-like commands to navigate both your directory of your local computer and the remote directory you’re connected to. To end your SFTP session, use the exit command. For a full list of SFTP commands, including those outside the scope of this guide, use the help command. 

[More Info](https://cat.pdx.edu/platforms/linux/remote-access/using-sftp-for-remote-file-transfer-from-command-line/)
