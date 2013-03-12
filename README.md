sqscmd
------

Helper for using SQS simply in bash scripting. Makes assumptions, like expecting your AWS credentials to be in `~/.s3cfg`.

    sudo npm install -g https://github.com/mapbox/sqscmd/tarball/master

    Usage:
      sqscmd <queue> get
      sqscmd <queue> put <data>
      sqscmd <queue> del <handle>

    Example:
      sqscmd 185391239583/testing put "Hello world"

Actual usage example:

    #!/bin/bash
    SQS="185391239583/testing"

    # Queue a URL to process.
    sqscmd $SQS put "http://example.com/to/download.tar.gz"

    # Retrieve queued message and store handle/body.
    RAW=$(sqscmd $SQS get)
    URL="$(echo "$RAW" | grep -oE "[^ ]+$")"
    HANDLE="$(echo "$RAW" | grep -oE "^[^ ]+")"

    # Process the item & remove from queue if successful.
    wget $URL && sqscmd $SQS del $HANDLE
