A manual of AsakusaSatellite
===================
AsakusaSatellite is [here](http://codefirst.github.com/AsakusaSatellite/)

Requirement
----------------
 * Sphinx 1.0.7 or later
 * Git 1.7 or later

Setup
----------------

    $ git submodule init
    $ git submodule update
    $ cd build/html/
    $ git checkout gh-pages

Update process
----------------

    $ vim source/index.rst
    $ make html
    $ cd build/html/
    $ git commit -a -m "Updated manual contents"
    $ git push origin gh-pages
    $ cd ../../
    $ git commit -a -m "Updated submodule"
    $ git push origin master

