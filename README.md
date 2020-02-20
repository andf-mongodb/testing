# testing

## macos_ulimit

### EITHER: Two `limit` files - Set limits at boot

1. Copy both `limit.*.plist` files to `/Library/LaunchDaemons/`

2. Enable with:

```
sudo launchctl load /Library/LaunchDaemons/limit.maxfiles.plist
sudo launchctl load /Library/LaunchDaemons/limit.maxprocs.plist
```

3. Run once with:

```
sudo launchctl start limit.maxfiles
sudo launchctl start limit.maxprocs
```

INFO:
- With `RunAtLoad` set, these two files will automatically run at boot. Test following a reboot with one of:

  ```
  sysctl -a | egrep '(maxfiles|maxproc)'
  launchctl limit
  ```

- I use the mongodb UNIX defaults of `64000` for both limits here. Edit to meet your needs.


### OR: One `mongodb` file - Set limits only for `mongod` when starting it

1. Copy the `com.mongodb-with-limits.plist` file to `/Library/LaunchDaemons/`

2. Edit the path to your `mongod` and `mongod.conf` file.

3. Enable with:

```
sudo launchctl load /Library/LaunchDaemons/com.mongodb-with-limits.plist
```

4. Run once with:

```
sudo launchctl start com.mongodb-with-limits
```

INFO:
- To apply these settings, you must use the above command to start your `mongod` each time. 
- I use the `brew` formula default of `4096` for both limits here. Not sure why they're different. Edit to meet your needs.
- I could not find a way to verify that these changes took effect for the `mongod` processes, but this is the same way we apply these setings in production with `brew`. YMMV.

### SOURCES:
- https://www.real-world-systems.com/docs/launchdPlist.1.html
