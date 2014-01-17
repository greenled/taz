taz
===

Script to daemonize a web application made with Play 2 framework

I am using this script in my Play 2 app [Pegotes](http://github.com/greenled/pegotes) to give sys admins an easier way to start/stop/restart the application as a system daemon. I have tested it in Ubuntu 13.04, feedback about other systems is welcome.

# Hot to use it

First you need to test the script to make sure it works with your app. To do so, package your play 2.2.x app as a Debian package and install it. Then copy `tazd` to `/etc/init.d/tazd` and rename it to whateves suits better for your app. Then edit the script and change header tags such as `Provides`, `Short-Description`, `Description` and values of variables such as `APP`, `DESC`, `NAME`, `DAEMON_NAME`, `DAEMON`, `PIDFILE`, etc. After changing the values and adjusting the script to your needs make sure it works executing `service tazd start`, `service tazd stop` and `service tazd restart` replacing `tazd` with the name you gift to the script.

Once you are sure it works, put it in the `src` directory relative to your project root (dev one, not the installed app) and add the following lines to your `build.sbt` again replacing `tazd` with the name of your script:

```
linuxPackageMappings in Debian <+= (name in Universal, sourceDirectory in Debian) map { (name, dir) =>
  (packageMapping(
    (dir / "tazd") -> "/etc/init.d/tazd"
  ) withUser "root" withGroup "root" withPerms "0644") asDocs()
}
```

Once added this to your `build.sbt` delete the testing script in `/etc/init.d` and uninstall the app. Then do `play reload`, `play clean`, generate an new Debian package and install it. You should be able now to manage your app like any other daemon with `service appd start/stop/restart` or `/etc/init.d/appd start/stop/restart`.

# Copyright

**Author**: Juan Carlos Mejías Rodríguez <juan.mejias@reduc.edu.cu>

Most of this script is part of a template script located in /etc/init.d/skeleton in [Ubuntu](http://www.ubuntu.com)

This script is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
