:title: Creating a `mai` profile for STUPS
:tags: mai, stups
:date: 2015-12-14
:slug: stups-creating-mai-profile
:category: stups
:status: published

The goal of *mai* tool in the STUPS suite is to provide authentication tokens to access AWS.

You have to create a *mai* profile and then log in into it, so you will get a token that will be used by other STUPS tools (e.g. *senza*). That token will last 1 hour. After that hour you will have to create a new one.

This is how I create a *mai profile*::

    $ ./venv-py3.4/bin/mai create <profile name>
    Identity provider URL: <e.g. https://aws.acme.net>
    SAML username: <e.g. joe@acme.net>
    Authenticating against https://aws.acme.net.. OK
    Please select one role
    1) AWS Account 123456789012 (acme-team1): AWS-Profile-Name
    2) AWS Account 210987654321 (acme-team2): AWS-Profile-Name
    Please select (1-2): 2
    Storing new profile in /home/pfigue/.config/mai/mai.yaml.. OK
    
If now I list that `mai.yaml` file I see several entries like this, one for each *mai* profile I created::

    profileName1:
      saml_identity_provider_url: https://aws.acme.net/
      saml_role: ['arn:aws:iam::210987654321:saml-provider/AWS-Profile-Name', 'arn:aws:iam::210987654321:role/AWS-Profile-Provider',
        acme-team2]
      saml_user: joe@acme.net

I can also do::      

    $ ./venv-py3.4/bin/mai create acme-team2 --url https://aws.acme.net/ --user joe@acme.net

  
Here is the `documentation for mai <http://stups.readthedocs.org/en/latest/components/mai.html>`_.