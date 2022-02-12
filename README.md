
AtomSpace CogStorage Server to LOGICMOO's Blackboard
===========================

opencog | singnet
------- | -------
[![CircleCI](https://circleci.com/gh/opencog/logicmoo_cogserver.svg?style=svg)](https://circleci.com/gh/opencog/logicmoo_cogserver) | [![CircleCI](https://circleci.com/gh/singnet/logicmoo_cogserver.svg?style=svg)](https://circleci.com/gh/singnet/logicmoo_cogserver)

The Logicmoo Cogserver is a network scheme/prolog command-line
and logicmoo server for the [OpenCog framework](https://opencog.org).

The code in this git repo allows an AtomSpace to communicate with
other AtomSpaces by having them all connect to a common CogServer.
The CogServer itself also provides an AtomSpace, which all clients
interact with, in common.  In ASCII-art:
```
 +-------------+
 |  CogServer  |
 |    with     |  <-----internet------> Remote AtomSpace A
 |  AtomSpace  |  <---+
 +-------------+      |
                      +-- internet ---> Remote AtomSpace B

```

Here, AtomSpace A can load/store Atoms (and Values) to the CogServer,
as can AtomSpace B, and so these two can share AtomSpace contents
however desired.

This provides a simple, straight-forward backend for networking
together multiple AtomSpaces so that they can share data. This
backend, together with the file-based (RocksDB-based) backend
at [atomspace-rocks](https://github.com/opencog/atomspace-rocks)
is meant to provide a building-block out of which more complex
distributed and/or decentralized AtomSpaces can be built.

This really is decentralized: you can talk to multiple servers at once.
There is no particular limit, other than that of bandwidth,
response-time, etc.  In ASCII-art:

```
 +-----------+
 |           |  <---internet--> My AtomSpace
 |  Server A |                      ^  ^
 |           |        +-------------+  |
 +-----------+        v                v
                 +----------+   +-----------+
                 |          |   |           |
                 | Server B |   |  Server C |
                 |          |   |           |
                 +----------+   +-----------+
```


Using
-----
There are multiple ways to start the logicmoo_cogserver: from a bash shell prompt
(as a stand-alone process), from the guile command line, or from the
prolog command line.

* From bash, just start the process:
  `$ ./prolog/logicmoo_cogserver.pl`


* From prolog: `use_module(library(logicmoo_cogserver).` and then
   `?- start_cogserver().` (where's the documentation for this?)

Once started, one can obtain a shell by saying `rlwrap telnet localhost
12101`, and then `pl` or `scm` to obtain a prolog or scheme shell.  This
can be done as many times as desired; all shells share the same
AtomSpace, and the system is fully multi-threaded/thread-safe.

The `rlwrap` utility simply adds arrow-key support, so that up-arrow
provides a command history, and left-right arrow allows in-place editing.
Note that `telnet` does not provide any password protection!  It is
fully networked, so you can telnet from other hosts. The default port
number `21001` can be changed; see the documentation.

Building and Running
--------------------

To install the network of assertions, just follow the next sequence of
commands in your SWI-Prolog shell:

```bash
	$ swipl
	
	?- pack_install('https://github.com/opencog/logicmoo_cogserver.git').
	true.
```

OR 

```bash
	clone https://github.com/opencog/logicmoo_cogserver
	cd logicmoo_cogserver
	swipl
	?- pack_install('.').
	true.
```


Prerequisites
-------------
To run the logicmoo_cogserver, you need to install the SWI-Prolog first.
logicmoo_clif, Logicmoo's Common Logic Interchange Format

Unit tests
----------
To build and run the unit tests, just say
```
    cd test
```

Architecture
------------
See also these README's:

* [network/README](opencog/network/README.md)
* [logicmoo_cogserver/README](opencog/cogserver/server/README.md)
* [builtin-module/README](opencog/cogserver/modules/commands/README.md)
* [cython/README](opencog/cython/README.md)
* [agents-module/README](opencog/cogserver/modules/agents/README.md)
* [events-module/README](opencog/cogserver/modules/events/README.md)


## known issues

- Only `scm` shell is supported for now. There are plans to extend this support to other offered shells in the future.
- The logicmoo agent/logic system can be very hard to install and is optional from here

## licensing information

This package, like the most of OpenCog packages, is licensed under [AGPL-3.0 license](LICENSE).
