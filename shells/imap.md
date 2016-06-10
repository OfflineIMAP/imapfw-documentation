

Playing with an IMAP repository is done by setting up a `shells.DriveDriver` shell.

``` python
# The "RemoteImap" repository.
class RemoteImap(types.Imap):
    conf = RemoteImapConf # Dict of the configuration.
    driver = drivers.Imap # Driver to use.


class DriveImapRemote(shells.DriveDriver):
    conf = {'repository': RemoteImap}

    def session(self):
        self.buildDriver()

        self.d.connect()
        self.d.login()
        self.interactive()
        self.d.logout()
```

At first, we configure the repository to use (here: `RemoteImap`). It has the
remote and credentials configured.

Next, we define what we want to do during the session. The `shells.DriveDriver`
has the `buildDriver()` helper to enable control of the IMAP driver via
`self.d` or `self.driver` (both are the same).

Internally, the IMAP driver (`drivers.Imap`) of the repository is run in a
worker (thread of process). So, `self.d` is a proxy to "remotely" control the
real driver.
