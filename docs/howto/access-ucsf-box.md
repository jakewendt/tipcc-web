# Access UCSF Box

It is possible to access [UCSF Box](https://ucsf.app.box.com/) using FTP over a secure SSL connection ([FTPS](https://en.wikipedia.org/wiki/FTPS)).

In order to do this, you need to setup a UCSF Box-specific password as 
explained in <https://ucsf.app.box.com/services/box_ftp_server>.  Afterward, you can use, for instance, the `lftp` client to verify access to your account;
```sh
$ lftp --user alice.aliceson@ucsf.edu ftps://ftp.box.com:990
Password: XXXXXXXX
lftp alice.aliceson@ucsf.edu@ftp.box.com:~> ls
drwx------  1 owner group     0 Jun 12  2014 Grant_R01
drwx------  1 owner group     0 Sep 30  2016 Secure-alice.aliceson@ucsf.edu
lftp alice.aliceson@ucsf.edu@ftp.box.com:~> exit
$ 
```

## Automatic authentication

When starting `lftp` as above, you need to manually enter your password, which can be tedious or even prevent automatic file transfers in batch scripts.  A solution to this is to set up the FTP credentials in `~/.netrc`.  Here is what it could look like:
```
$ cat ~/.netrc
machine ftp.box.com
	login alice.aliceson@ucsf.edu
	password AliceSecretPwd2017
```

**Since the password is fully visible in plain text, make sure to keep this file private at all times**, otherwise users on the system can see all your credentials, i.e.
```sh
$ chmod 600 ~/.netrc
$ ls -l ~/.netrc 
-rw------- 1 alice alice 72 Jul  3 15:10 /home/alice/.netrc
```

To verify that the automatic authentication works, try to login again. You should no longer be prompted for your password - instead `lftp` gets it automatically from `~/.netrc`.  For example:
```sh
$ lftp --user alice.aliceson@ucsf.edu ftps://ftp.box.com:990
lftp alice.aliceson@ucsf.edu@ftp.box.com:~> ls
drwx------  1 owner group     0 Jun 12  2014 Grant_R01
drwx------  1 owner group     0 Sep 30  2016 Secure-alice.aliceson@ucsf.edu
lftp alice.aliceson@ucsf.edu@ftp.box.com:~> exit
$ 
```