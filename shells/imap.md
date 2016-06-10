

Playing with an IMAP repository is done by setting up a `shells.DriveDriver` shell.

``` python
# The "RemoteImap" repository.
class RemoteImap(types.Imap):
    conf = RemoteImapConf # Dict of the configuration.
    driver = drivers.Imap # Driver to use.


class DriveImapRemote(shells.DriveDriver):
    conf = {'repository': RemoteImap}

    def session(self):
        self.d.connect()
        self.d.login()
        self.interactive()
        self.d.logout()
```

At first, we configure the repository to use (here: `RemoteImap`). It has the
remote and credentials configured.

Next, we define what we want to do during the session. The `shells.DriveDriver`
has the `buildDriver()` helper which is called at `beforeSession` time. This
enables the control of the IMAP driver via `self.d` or `self.driver` (each is an
emitter to the driver).

Internally, the IMAP driver (`drivers.Imap`) of the repository is run in a
worker (thread of process). So, `self.d` and `self.driver` allow to "remotely"
control the real driver.


## Session sample

``` python
nicolas@home> ./imapfw.py -r rascals/sample.rascal shell DriveImapA
INFO    : RemoteImap driver ready!

Welcome to the shell! The driver is running in a worker. Take control of it with
the pre-configured emitter. It is available from both the "driver" and
"d" variables.  "d" will send any event in sync mode.  Ctrl+D: quit

Available commands:
- events(): print available events for the driver.

Example:
>>> d.help()

The driver was already built in the default beforeSession() method of this
shell.

>>> d.help()
Available events:
- buildDriver: Build the driver object in the worker from this account side.
- buildDriverFromRepositoryName: Build the driver object in the worker from this
  repository name.

The repository must be globally defined in the rascal.
- connect: None
- getCapability: None
- getClassName: None
- getDriverClassName: None
- getFolders: None
- getNamespace: None
- getRepositoryName: None
- init: None
- isDriverBuilt: None
- isLocal: None
- login: None
- logout: None
- search: None
- select: None
>>> d.isDriverBuilt()
True
>>> driver.getFolders()
>>> driver.cached_getFolders()
['INBOX', 'INBOX/spam', 'INBOX/outbox', 'INBOX/sp&AOk-cial']
>>> d.getFolders()
['INBOX', 'INBOX/spam', 'INBOX/outbox', 'INBOX/sp&AOk-cial']
>>> d.logout()
True
>>> <CTRL+D>
nicolas@home>
```
