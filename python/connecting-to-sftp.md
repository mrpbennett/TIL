# Connecting to SFTP via Paramiko

Using [Paramiko](https://docs.paramiko.org/en/stable/index.html) to connect to
an SFTP is pretty easy after some digging around. This is how I have done it via
the use of [Transport](https://docs.paramiko.org/en/stable/api/transport.html).
Once connected you can simply use the
[SFTP](https://docs.paramiko.org/en/stable/api/sftp.html) client to do what is
needed.

Below I connect to the SFTP of my choice, change to a certain directory then
upload a file to that directory before closing the connection.

```python
def sftp_test(change_of_dir: str, localpath: str, remotepath: str):
    try:
        transport = paramiko.Transport(host, 22)
        transport.start_client()
        transport.auth_password(username, password)

        sftp = paramiko.SFTPClient.from_transport(transport)

        sftp.chdir(change_of_dir)
        sftp.put(localpath, remotepath)

        sftp.close()

    except Exception as err:
        print(f"Could not connect SFTP client: {err}")
        raise err
```
