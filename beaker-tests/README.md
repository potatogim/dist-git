How to run tests
================

The following command will setup your system, setup a Fedora virtual machine and run tests from the `./tests/` subdirectory. You need to be root and the command will modify your system, i.e. install some packages, overwrite `/etc/rpkg/*.conf` configs and `/etc/rpkg.conf`, write an entry into `/etc/hosts`, and generates root ssh public key if not generated yet. It might be therefore a good idea to run this command inside another virtual machine (try with nested virtualization enabled) or a container:

```
# ./run.sh
```

The setup part for host system modifications and creation of the virtual machine is done by `./setup.sh` script that is invoked from `run.sh` and is the part that requires root privileges. Use `cat setup.sh` to see what the script does. You can choose not to run the script during `./run.sh` execution if you have run it before (either through `./run.sh` or `./setup.sh` directly). To do this, pass `-x` switch to `./run.sh`:

```
# ./run.sh -x
```

To run the tests for CentOS7 (with epel7 repos enabled) instead of Fedora, you can use:

```
# DISTGIT_FLAVOR=dist-git-centos-7 ./run.sh
```

This will spawn a new CentOS7/opel7 virtual machine.

To do the same for CentOS8 (with epel8 repos enabled), you can use:

```
# DISTGIT_FLAVOR=dist-git-centos-8 ./run.sh
```

Between each run, you need to remove `pkgs.example.org` record from `/root/.ssh/known_hosts` and you should also clean-up `/etc/hosts` from stale `pkgs.example.org` records.

Once the virtual machine you want to test exists and once it is correctly pointed to by `pkgs.example.org` ip record in `/etc/hosts`, you can again run just `./run.sh -x` to skip the `./setup.sh` invocation.

At this point, you can also run just a specific test by switching into `./tests/<testname>` subdirectory and invoking `./run.sh` there. Alternatively, you can pass `-r <testname>` together with `-x` switch to the master `./run.sh` script (the one next to this README):

```
# ./run.sh -x -r fedpkg-test
```

If you want to look inside the created virtual machines, switch to the git top-level directory and run `vagrant ssh <dist-git-flavour>` command there as root, e.g.:

```
# vagrant ssh dist-git
```

to switch to the Fedora virtual machine.

Or

```
# vagrant ssh dist-git-CentOS-8
```

to switch to CentOS-8/epel-8 virtual machine.