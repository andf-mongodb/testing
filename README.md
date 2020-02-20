# testing

## macos_ulimit

### EITHER: Two `limit` files

1. Copy both "limit" files to `/Library/LaunchDaemons/`

2. Enable with:
```
sudo launchctl load limit.maxfiles
sudo launchctl load limit.maxprocs
```

3. Run once with:
```
sudo launchctl start limit.maxfiles
sudo launchctl start limit.maxprocs
```

INFO:
- With `RunAtLoad` set, these two files will automatically run at boot. Test with `ulimit -a` after a reboot.
- I use the mongodb UNIX defaults of `64000` for both limits here.

### OR: One `mongodb` file

1. Copy the file to `/Library/LaunchDaemons/`

2. Edit the path to your `mongod` and `mongod.conf` file.

3. Enable with:
```
sudo launchctl load com.mongodb-with-limits
```
4. Run once with:
```
sudo launchctl start com.mongodb-with-limits
```
INFO:
- With `RunAtLoad` disabled, you must use the above command to start your `mongod` each time. 
- I use the `brew` formula default of `4096` for both limits here. Not sure why they're different. Edit to meet your needs.
- I could not find a way to verify that these changes took effect for the `mongod` processes. YMMV.
