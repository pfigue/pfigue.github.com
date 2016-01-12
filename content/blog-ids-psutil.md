title: Looking for listening services on my local with Python
tags: python, psutil, netstat, ports listening
date: 2016-01-08
slug: listening-services-python-psutil
authors: pfigue
category: python
status: published

Every now and then I have to run `netstat -tlpn` on my machine and I discover some processes listening there, for everybody who wants to connect.

Sometimes it is an MySQL, sometimes an ElasticSearch. Nothing that I want to have there available for everybody.

I wanted to automate some solution to warn me when something new starts to listen there, so the first I though was about developing some wrapper around `netstat` command with python, but accidentaly I discovered the [psutil module](https://pypi.python.org/pypi/psutil).

So, here is a proof of concept for *inet* sockets (*ip4* and *ip6*):

    import psutil
    import socket

    rows = []
    lc = psutil.net_connections('inet')
    for c in lc:
        (ip, port) = c.laddr
        if ip == '0.0.0.0' or ip == '::':
            if c.type == socket.SOCK_STREAM and c.status == psutil.CONN_LISTEN:
                proto_s = 'tcp'
            elif c.type == socket.SOCK_DGRAM:
                proto_s = 'udp'
            else:
                continue
            pid_s = str(c.pid) if c.pid else '(unknown)'
            msg = 'PID {} is listening on port {}/{} for all IPs.'
            msg = msg.format(pid_s, port, proto_s)
            print(msg)
        
Which throws these results now on my machine:

    PID 7162 is listening on port 5353/udp for all IPs.
    PID 29784 is listening on port 7534/tcp for all IPs.
    PID (unknown) is listening on port 68/udp for all IPs.
    PID (unknown) is listening on port 631/udp for all IPs.
    PID (unknown) is listening on port 30158/udp for all IPs.
    PID (unknown) is listening on port 9200/tcp for all IPs.
    PID (unknown) is listening on port 38047/udp for all IPs.
    PID 13932 is listening on port 48854/tcp for all IPs.

It is using the [`psutil.net_connections`](http://pythonhosted.org/psutil/#psutil.net_connections)  function.

This is accurate as long as the information source (I guess the kernel, but I don't know how is it implemented, if reading `/proc/` data or what...) is trustable. If there is some rootkit providing false information, then it would be better to do some port scanning instead of reading this info.