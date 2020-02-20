# testing

## macos_ulimit - Set `maxproc` and `maxfiles` at boot.

1. Copy both `limit.*.plist` files to `/Library/LaunchDaemons/`

2. Enable with:

   ```
   sudo launchctl load /Library/LaunchDaemons/limit.maxfiles.plist
   sudo launchctl load /Library/LaunchDaemons/limit.maxproc.plist
   ```

3. Run once with:

   ```
   sudo launchctl start limit.maxfiles
   sudo launchctl start limit.maxproc
   ```

INFO:
- With `RunAtLoad` set, these two files will automatically run at boot. Test following a reboot with one of:

  ```
  sysctl -a | egrep '(maxfiles|maxproc)'
  launchctl limit
  ```

- I use the mongodb UNIX default of `64000` for `maxfiles` here, but for `maxproc`, macOS seems to reject the suggested UNIX default of `64000`. It even rejected `4096` which is what `brew` attempts to set. I use `2128` instead. Edit to meet your needs.

### SOURCES:
- https://www.real-world-systems.com/docs/launchdPlist.1.html
