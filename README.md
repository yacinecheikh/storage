(TODO: licence en premier)
(GPL)


# Storage

A pass-like encrypted and remotely synchronized file storage

## Basic useage

`storage` uses GPG to encrypt stored files. To define the GPG secret key to use, run:
```
gpg --gen-key  # create a new GPG key
storage key mykey@gmail.com  # save the GPG key for this email address
storage key  # display the GPG key fingerprint
```

When creating a GPG key, you should always use a passphrase to prevent someone with physical access to any of your machines from accessing your private files.


## Storing and retrieving files

The following will save `bill.pdf` as `bills/<current-date>.pdf.gpg` under the storage file store.
```
storage save bill.pdf bills/$(date -I).pdf
```

To retrieve the file:
```
storage get bills/2024-11-18.pdf bill.pdf
```

If you do not need it anymore:
```
storage rm bills  # rm -r ~/.file-store/bills(.gpg)
```


## Server synchronization with SSH

You can also configure a remote server with SSH access to synchronize your files before any accident:
```
storage server vps:file-store  # must be a valid scp url (<ssh server>:<path>)
storage server  # display the configured server
```

To synchronize:
```
storage pull  # override the local storage with the server:
```
```
storage push  # erase the remote storage and replace it with the local files
```

`storage` does not feature versioning like pass, because manipulating non-textual files with git would lead to a larger and larger git history over time. Instead, you can use the server and other synchronized machines as "delete-safe" backups for when you accidentally remove an important file.


## Export encrypted files and GPG keys

In order to synchronize a new machine with your `storage` server, you need to:
- install gpg, ssh and storage
- configure ssh keys for prompt-less connections
- configure the `storage` server url
- import the GPG public and secret keys on the new machine
- configure the `secret` key

To export your GPG keys:
```
gpg --export --armor <myemail> > myemail.gpg
gpg --export-secret-keys --armor <myemail> > myemail_secret.gpg
```


To import on the new machine:
```
gpg --import myemail.gpg
gpg --import myemail_secret.gpg
```
