discredit
=========

discredit is a distributed text editor based on Ace and ShareJS.

The idea is that you can run discredit on a machine and point it to a
directory.  That directory, along with a text editor, is served over
http. On another machine, one can load the URL to edit the files.


Install
-------

First, grab the latest release of Node.js from http://nodejs.org/ and
install it. At the time of writing, the latest version of Node.js in
Ubuntu is too old.

Next, clone this repo to the server machine.

`git submodule init && git submodule update`

Then, go inside and get the dependencies for this and for Ace:

`npm install && cd ace && npm install`

Now, build the project:

`./build`

Run
---

Start the server:

`[NODE_PATH=/usr/local/lib/node_modules] node src/server2.js [-r path/to/root/folder]`

Then in a browser window, visit the URL:

http://[host:port]/test.html


How it works
------------

ShareJS serves documents, each with a given name. It backs them by memory
(ShareJS can back items to a database, but that feature is not used by discredit).
When discredit starts, it runs a ShareJS server which starts out with no documents,
it serves up static files (the editor), and it listens for commands from clients.

When the client loads the URL, it gets the editor and can start viewing file lists.
When a file is clicked, a command is called to create the document on the ShareJS server,
and a new special client is created on the server that syncs the ShareJS document to disk.

The client then loads the Ace editor and connects it to the ShareJS client code.

On the server, there is the special client which syncs the ShareJS document to disk.
Whenever the ShareJS document changes, it's contents are written to disk.
Whenever the disk contents change, the new contents are compared with the existing conents
with a file differ. The diff is then applied to the ShareJS document, thus causing it to
match the disk.

