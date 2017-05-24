A Puppet pre-commit hook
========================

This is inspired/editted from the pre-commit hook found on the following blog: http://techblog.roethof.net/puppet/a-puppet-git-pre-commit-hook-is-always-easy-to-have/

All credits should really go to Ronny Roethof, I merely editted it and added some ERB template checking.

The code is made available here under the MIT license.

What does this do?
-----------------

For every commit you make into your repository, this script will check the syntax of your code (both Puppet (.pp) and Ruby Templates (.erb)) to make sure they are valid. This does not check the actual workings of your Puppet code, you should look into rspec or cucumber testing for that.

If any invalid syntax is found, it will abort the commit and you won't be able to commit your faulty code that will break production. Yay!

Installation
------------

Go to your git repository, create the file .git/hooks/pre-commit and place the content of this repo in it. Make it execute (chmod +x $file).

There is also a simple installer provided called `install`. Provide a directory that contains your puppet repositories (default: `..`). Running this will attempt symlink the `.git/hooks/pre-commit` to this hook in all directories at the specified location without regard to whether it is a valid git repository. This may simplify installation when you have multiple puppet repos in the same directory.

```
$ ./install ~/git/puppet/
Installing hook to all directories underneath /home/rnelson0/git/puppet/
Adding hook to directory controlrepo
Adding hook to directory custom_facts
Adding hook to directory profile
Adding hook to directory role
Setting the executable bit on the hook.
Installation complete.

```

What does it look like ?
------------------------

Every commit will check the changed files and will report on them as such.

<pre> $ git commit modules/name/ -m "Your commit message"

*** Checking puppet code for style ***
PASSED: modules/name/manifests/init.pp
PASSED: modules/name/tests/init.pp

*** Checking puppet manifest syntax ***
PASSED: modules/name/manifests/init.pp
PASSED: modules/name/tests/init.pp

*** Checking ruby template(erb) syntax ***
PASSED: modules/name/templates/template-file.erb

No Errors Found, Commit ACCEPTED
[develop adb6889] Your commit message
 3 files changed, 120 insertions(+)
  create mode 100644 modules/name/manifests/init.pp
  create mode 100644 modules/name/templates/template-file.erb
  create mode 100644 modules/name/tests/init.pp
</pre>
