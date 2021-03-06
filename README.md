EddyRespondek's version doesn't work. This is an attempt to fix it *DONE*

Now I am trying to make it work with 1.8.7 *DONE Check below for more instructions*

---------------------------
MoinMoin on OpenShift
=====================

This git repository helps you get up and running quickly with a single Wiki 
installation of MoinMoin on OpenShift.


Quickstart
----------------------------

Create an account at http://openshift.redhat.com/

Create a python-2.6 application

    rhc app create -a moinmoin -t python-2.6

Add this upstream repo

    cd moinmoin
    git remote add upstream -m master git://github.com/friedrichg/moinmoin-openshift-example.git
    git pull -s recursive -X theirs upstream master

If you want to try moinmoin 1.8.7 (instead of moinmoin 1.9) run this:

    cp .openshift/action_hooks/build-1.8 .openshift/action_hooks/build
    cp setup.py-1.8 setup.py
    cp wsgi/application-1.8 wsgi/application
    
Then push the repo upstream

    git push

That's it, you can now checkout your application at:

    http://moinmoin-$yournamespace.rhcloud.com
    

What's going on?
----------------------------

MoinMoin uses a file-system based data storage backend rather than a 
tradition SQL database. When you push this application up for the first
time the base data files are copied to Openshift's data directory for 
persistent storage.

A quirk of MoinMoin is that plugins are also kept in the same data 
directory mentioned previously, which isn't very handy since you'll
more than likely want to modify these files to create custom themes, etc.
A copy of the plugins directory has been placed in 
~/moinmoin/repo/wsgi/plugin and will overwrite the plugin folder in the
data directory everytime the application is pushed up.

All of MoinMoin's default static files have been moved to
~/moinmoin/repo/wsgi/static.

An example MoinMoin logging configuration file has also been added and can
be found in ~/moinmoin/repo/wsgi/logging. The resulting log file is 
located in Openshifts log directory and can be viewed using Openshift's rhc
client using the tail command.
