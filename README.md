# Threadsafe Pub

Simple wrapper to use PyPubSub in a multithreaded environment, e.g.
when using a GUI with a mainloop in a main thread and some worker threads that
want to publish some data back into the main thread.

Inspired by [this thread](http://groups.google.com/group/pypubsub_dev/browse_thread/thread/7f414e82f62d64b7).
Uses a threadsafe Queue.queue to collect messages


Use this module like the "pub" module in pubsub: Just import it in every module you want to
use it, it will automatically create a "ThreadSafePub" instance at import.

## Example

The worker threads can do this:

```
#!python

from threadsafepub import pub as tpub

tpub.sendMessage("someTopic", somedata="Some Data")
```


The main thread can do this:

```
#!python

from threadsafepub import pub as tpub
```

and in its idle callback:
```
#!python

mainThread_IdleCallback():
    tpub.poll()
```

Note:
The listeners just subscribe to pub.subscribe as usual! This module doesn't provide a
thread-safe subscription method. It just calls pub.sendMessage for all queued messages when
poll() is called.
