# Glock
Go-implemented Global Lock.

## Rationale
[Flock](http://man7.org/linux/man-pages/man1/flock.1.html) provides a mutual exclusion lock built on the filesystem.

Glock also provides a mutual exclusion lock but, by using ZooKeeper, provides a global lock suitable for use across machines.

## Usage

```
$ glock --zookeeper=localhost:2181 --path=/glock/some/task echo 'hello, world'
```

The `glock` command will first attempt to acquire a lock from ZooKeeper within `wait` (default 5 seconds). The process that acquires the lock will immediately execute the command.

## Notes

Glock is intended to be used as a short-lived process. That is, it is intended to be used from within Crontab or other such systems. To ensure locks are properly acquired across machines it's necessary to set `wait` and `minExec` to reasonable values to cover the difference in clocks: if the command executes faster than the difference between clocks there's a chance a task would execute twice in the same cycle.

## Building

Glock dependencies were managed using [Govendor](https://github.com/kardianos/govendor). You can specify `GO15VENDOREXPERIMENT=1` to pull dependencies from the `./vendor` directory.

## License

BSD 3-clause. Please see `LICENSE`.
