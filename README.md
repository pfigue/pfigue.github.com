# Installation log

I created the virtualenv with:

  virtualenv --python=/usr/bin/python3.5 venv-c3.5/

Then set up packages with:

  ./venv-c3.5/bin/pip install pelican Markdown

Then ran:

  ./venv-c3.5/bin/pelican-quickstart

then commit and pushed to bitbucket.

# Rendering output

In the blog root directory:

  $ rm -rf output/
  (Tue Jan 12 10:55:31)0 pfigue@perezoso:~/Workspace/blog-pfigue/
  $ ./venv-c3.5/bin/pelican -o output/ content/
  Done: Processed 6 articles, 1 draft, 0 pages and 0 hidden pages in 0.30 seconds.

# Publishing

  $ ./venv-c3.5/bin/ghp-import -r publish -b master -p output/

If the remote `publish` does not exist:

  $ git remote add publish ...

