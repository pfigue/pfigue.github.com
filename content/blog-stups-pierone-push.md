title: Logging in into the STUPS docker registry (Pier One)
date: 2015-12-14
tags: stups, pierone
slug: stups-logging-pierone
category: stups
status: published

I log in as *johndoe*:

    $ ./venv-py3.4/bin/pierone login --user johndoe
    Getting OAuth2 token "pierone".. OK
    Storing Docker client configuration in /home/jdoe/.docker/config.json.. OK

This has written some Docker configuration:

    $ cat  ~/.docker/config.json | jq '.'
    {
      "auths": {
        "https://pierone.repo": {
          "email": "no-mail-required@example.org",
          "auth": "b2F1dGgyOjQ4NTEyODZiLTZmZTctNTBlYy05MjAxLTlhNzkwNjM5NzQzMA=="
        },
        "null": {
          "email": "no-mail-required@example.org",
          "auth": "b2F1dGgyOjM2NTEyODZiLTUwNDUtZWIyMy05MjAxLWVhNzYwNmZiNzIzMQ=="
        }
      }
    }

Tagging the image *04d55adc8567* before pushing:

    $ sudo docker tag 04d55adc8567 pierone.repo/teamName/imageName:0.1-VERSION

Pushing to the Docker Hub:

    $ sudo docker push pierone.repo/teamName/imageName:0.1-VERSION
    The push refers to a repository [pierone.repo/teamName/imageName] (len: 1)
    04d55adc8567: Image already exists 
    [...]
    59fcbbb9028a: Image already exists 
    0.1-VERSION: digest: sha256:7c81309cc089f80525fd777788c8401ec9a37153e8b98dd8e9ef3231440653da size: 8500