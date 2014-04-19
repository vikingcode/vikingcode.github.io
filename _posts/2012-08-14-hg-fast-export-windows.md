---
title: Hg to Git on Windows
layout: page
published: true
---

A few times I've had issues wanting to convert Hg to Git. Hg-fast-export is probably the best/fastest way, but on Windows its a pain in the ass.

The full details of *why* its a pain in the ass can be found on [Mehdi](http://www.mehdi-khalili.com/migrating-from-mercurial-to-git)'s blog, but for my own benefit I'm posting this here so next time I need to convert I don't have to wait until I get up to the error before being able to find his blog again!

1. Install [Python 2.6](http://www.python.org/download/releases/2.6.6/) (yes, specific versions because compatibility is shit)
2. Install [Mercurial for Python](https://bitbucket.org/tortoisehg/thg-winbuild/downloads/mercurial-2.2.2.win32-py2.6.exe)
3. Add python to PATH (`C:\python26` by default) 
4. `git clone git://repo.or.cz/fast-export.git`
5. modify `hg-fast-export.py` to have the following up the top

{% highlight python %}
	import sys

	if sys.platform == "win32":
		import os, msvcrt
		msvcrt.setmode(sys.stdout.fileno(), os.O_BINARY)
{% endhighlight %}    

6. Finally do the rest of the commands.  

		mkdir new_git_repo
		cd new_git_repo
		git init
		/path/to/hg-fast-export.sh -r /path/to/hg_repo
		git checkout HEAD