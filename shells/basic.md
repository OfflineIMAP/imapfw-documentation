
Users can enable shells in the rascal from the basic shell `shells.Shell`.

``` python
class MyShell(shells.Shell):
    def beforeSession(self):
        """Setup the environment."""

        # Tune the banner which is printed before the session.
        setBanner("My shell to play with a message.")

        # Create a message object.
        self.message = types.message.Message(2)

        # Register the object to get access to it during the session. The
        # registered variable must be an attribute (`self.variable_name`).
        self.register('message')

    def session(self):
        """The session to run."""

        # Show the message.
        print(self.message)

        # Enter interactive mode.
        self.interactive()

    def afterSession(self):
        """Code to run when at the end of the session."""

        print("Session stopped.")
```

So in the rascal, we define the shell name (here `Myshell`) and what the shell
does:

* `beforeSession`: prepare a session.
* `session`: run pre-defined code (non-interactive) or enter in interactive
  mode.
* `afterSession`: what to do once the session is done.
