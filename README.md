# gradle-remote - a simple way to compile your projects on a remote machine

If you have more bandwidth than computing power, it's very convenient to be able to compile and even run tests on a remote server. `gradle-remote` is a simple script that you can use in place of the `gradle` command.

It will upload your project to the remote server, run the specified gradle command and then download the results back to your local build directory. Since it uses `rsync` to transfer files, the file changes will be delta compressed - in practice the uploads are very fast even for large projects.

## Configuration

Set GRADLE_REMOTE_SSH to a valid ssh login target. For example `myserver.net` or `gradle_builder@example.com`.

Set GRADLE_REMOTE_DIR to the base directory in which projects will be stored. You probably want a dedicated user on the server for running the builds, so something like `/home/gradle/projects` will work.

If you wish to delete inactive projects after some time, use a simple cron job:

```
0 * * * * find /home/gradle/projects -mindepth 1 -maxdepth 1 -type d -mmin +720 -print0 | xargs -0 rm -rf
```

## Future ideas

* Smarter logic for excluding files from uploads
* Utilizing a FUSE-based remote filesystem
