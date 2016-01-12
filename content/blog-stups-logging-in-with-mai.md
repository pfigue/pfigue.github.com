title: Log in with STUPS `mai` into AWS.
date: 2015-12-14
tags: stups, mai
slug: stups-logging-with-mai
category: stups
status: published

Before log in with *mai*, we can list which profiles we have:

    $ ./venv-py3.4/bin/mai list                                                                                                                                                                                  
    Name             │Role                                                             │Url                     │User                      
    profileName1    AWS Account 288593816804 (acme-team2): AWS-Profile-Name     https://aws.acme.net joe@acme.net 

ok, now I know I want to log in into the `profileName1` profile:

    $ ./venv-py3.4/bin/mai login profileName1
    Authenticating against https://aws.zalando.net/.. OK
    Assuming role AWS Account 210987654321 (acme-team2): AWS-Profile-Name.. OK
    Writing temporary AWS credentials.. OK

This seems to write something here:

    cat ~/.config/mai/last_update.yaml
    {profile: profileName1, timestamp: 1450084393.4453733}
    
I don't know what happens when I log in in several profiles at the same time.
