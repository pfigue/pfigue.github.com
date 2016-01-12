title: Personal notes - Notizen
date: 2015-12-09
tags: notizen, project
category: notizen
authors: pfigue
status: published

Since a couple of months I use to write down into files my findings whenever I research a subject at work (sometimes also at home).

I keep all those notes in a [git repo](https://github.com/pfigue/notes), so I can manage them easily (different computers, versioning, publishing, etc.).

# Classifying

At some point, when the number becomes big I lost track of which notes I have and where. That's why I decided to start tagging them so I could write some automation to look for my notes in a given subject.

A very simple example of note:

    Testing in python
    tags: python, testing
    
    here comes the content of the note.
    
For [more examples of notes](https://github.com/pfigue/notes) you can check the repo, but I think the idea is clear: there are some fields (like `tags:`) which are used to classify and index the notes.

# Automating: Notizen

Recently I started developing some python thingies that:

  1. Index the notes with their tags
  2. Can perform searches for a given tag
  
All of these under the name *Notizen*. I will be publishing that [python package](FIXME) soon, both in the [Cheeseshop](FIXME) and in [bitbucket](https://bitbucket.org/pfigue/notizen).

# Refs

1. PyPI [notizen package](FIXME).
2. Bitbucket [notizen repo](https://bitbucket.org/pfigue/notizen).
3. The [notes in Github](https://github.com/pfigue/notes).
