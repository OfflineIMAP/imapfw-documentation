imapfw includes shells. They are very usefull for a lot of use cases like
hacking on imapfw, debugging, introspecting, learning IMAP, etc... A shell
allows to run a manual session and play with high-level objects.

``` bash
> imapfw shell -h
usage: action shell [-h] SHELL_NAME

positional arguments:
  SHELL_NAME  the shell from the rascal to run

optional arguments:
  -h, --help  show this help message and exit
>
```

The shells can be used in 3 ways:
* interactive mode;
* non-interactive mode;
* a mix a both.

A shell is started with a simple command line like:

``` bash
imapfw shell MyShell
```
