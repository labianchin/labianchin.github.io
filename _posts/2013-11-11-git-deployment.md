---
layout: post
category : tools
tags : [git, ci, cd, webserver, tools]
---


## Using git for production deployment

This week I have made a git setup for deployment. I would like to share the way I did. Feel free to comment about it.

So, when doing web development (e.g. PHP, Ruby, Python, …) with dynamic languages, what you really need to put the code in production is (generally) just copy to the production server. You could do that by using SCP, rsync or just using git and pulling the changes from inside the server.

What if you could just push your changes to the server and automacaly get your changes live?

The idea is that you could have a git remote pointing to the production server. So, when you push to this remote, it would automacally update the production server.

This is very similar with how one deploy to Heroku (https://devcenter.heroku.com/articles/git)

### How to do it

Say you have the following repository on Github:

	git@github.com:luisarmando/demo.git

In the production server create a mirror for this repository:

	$ git clone --mirror git@github.com:luisarmando/demo.git ~/demo.git

The actual production source code would be a clone for that mirror, also lets name this remote as "hub":

	$ git clone ~/demo.git --origin hub /var/www/demo

Then we setup a post-update hook on the hub repository (mirror). Edit the file ~/demo.git/hooks/post-update:

	#!/bin/sh
	ROOT=/var/www/demo

	echo
	echo \"**** Pulling changes into Live [Hub's post-update hook]: $ROOT\"
	echo

	cd $ROOT || exit
	unset GIT_DIR
	git pull hub

	exec git-update-server-info


Now on your local git repository you can add a new remote to this hub repository:

	$ git remote add production user@production-server:/home/user/demo.git

And everytime you push to *production* you will be updating the production server.
	
	$ git push production master

PS: You can also run *git remote update* on the hub repository, so that you sync with your github repository.

### Final Remarks

Now you can both push to origin to share the code with the team, or accually push to production.

Find more details in:

- [A web focused git workflow](http://joemaller.com/990/a-web-focused-git-workflow/)
- [Using git for deployment](http://danbarber.me/using-git-for-deployment/)

You might also want to check some [continuous delivery](http://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912) practices. However this idea makes pushing code to production very easy, it would be nice to consider having a continuous integration (CI) pipeline with several tests. Also, at the end of the CI pipeline an "artifcact" with the production code would be generated and deployed to production.