# Customize and build your base installers
This page describes the customizations to code required before you can build base installers, and also the actual build process. We assume you have [installed](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Installation.md) and [configured](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Configuration.md) the Custom Installer Builder already.

-----

## Set nodemanager version
In `~/custominstallerbuilder/DEPENDENCIES` there is a subdirectory for the `nodemanager`. Go there, and edit `nmmain.py`. It includes a line starting with `version = `.

Change the version string to reflect your project / clearinghouse / Custom Installer Builder name, and also the current build. You will later need this string when launching the build script.

**NOTE: This string will be used as a part of the base installer's file name. Use only printable, non-whitespace, ASCII characters that do not require escaping! ** Avoid tabs and spaces, forward and backslashes (`/
`), quotes/ticks of all kinds (`'"`), shell glob characters (`?`, `*`), and other characters with special meanings to the shell (`#&><|()[]{}!$;~` etc.).

A-Z, a-z, 0-9, `.` (period), `-` (dash), `_` (underscore) are fine.


## Check softwareupdater URL and key pair
Remember the public/private key pair whose filenames and paths you [configured](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Configuration.md#adapt-the-build-scripts) in the build script? In order for your installers to be able to validate updates that you push, the `softwareupdater` component must include your (or your update site's) public key.

For this, change into the `softwareupdater` subdir of `~/custominstallerbuilder/DEPENDENCIES/` and edit `softwareupdater.py`. On the line starting with `softwareupdatepublickey = `, replace the value under the key `'e'` of the dict with the first `int` of your public key, and the value of key `'n'` with the second.

Furthermore, change the line starting with `softwareurl = ` to contain your softwareupdater's URL.
```python
softwareurl = "http://blackbox.poly.edu/updatesite/"
```
Done? Great, because that's all of the customization required! Let's build installers now!



## Building base installers

First, change to a user account with `sudo` privileges.

Go to `/home/cib/custominstallerbuilder/` where `rebuild_base_installers.sh` resides. Run the script, supplying it the nodemanager `version` string you set earlier.

```sh
$ ./rebuild_base_installers.sh YOUR-VERSION-STRING
```

After a bit of processing and signing files with the softwareupdater's cryptographic key, the base installers are ready in the `$BASE_INSTALLER_DIRECTORY` you set (`/var/www/dist` per default).



## Next up...

Continue now to [testing](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Testing.md) or [production deploying](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Deployment.md) the Custom Installer Builder.