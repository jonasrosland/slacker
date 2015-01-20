# Slacker 
## A neck-beardy chatops bot for the discriminating Ops professional

Written in Go for SlackHQ (using the RTM API), Slacker provides features that
make it easy to model and abstract many different types of interaction between
engineers and production environments. 

 * Define Event and Message [Handlers](docs/handlers.md) so Slacker will parse and respond to chat conversation and commands
 * Specify Cron-Syntax Schedules for Slacker to run periodic [*Chores*](docs/chores.md)
 * Set up inbound and outbound [filters](docs/filters.md) to modify events enroute to or from SlackHQ
 * Correctly handle and respond to system-level events like SigTerms by configuring runtime [hooks](docs/hooks.md)
 * Verbose Logging 
 * Effecient, parallel execution of handlers, filters, and chores
 * Simple semantics for creating and linking-in your own plugins
 * Full access to all Slack RTM events and [metadata](docs/meta.md)

That's all working now (more documentation to come).  Features I'm currently working on include: 

 * In-memory and redis-backed persistent storage (a brain)
 * Slack slash-command support (to send private commands)
 * failover
 * more documentation

## Check out Slacker from your workstation in 5 minutes

1: Select *Configure Integrations* from your team menu in slack

2: Add a new *Bots* integration, give your bot a clever name, and take note of your Token

3: 
```
	go get github.com/djosephsen/slacker
```

4: 
```
	export SLACKER_NAME=<whatever you named your bot in the Slack UI>
	export SLACKER_TOKEN=<your token>
	export SLACKER_LOG_LEVEL=DEBUG  # (optional if you'd like to see verbose console messages)
```

5: 
```
slacker
```

At this point you should see slacker join your default channel and say hi. 

![hi](docs/screenshots/hi.png)

If you ctl-C him in the console window, you should also see him say goodbye
before he leaves the channel. 

![bye](docs/screenshots/bye.png)

These behaviors are both implemented as inithooks (the first as a startup hook
and the second as a shutdown hook). If you find this annoying you can comment
them out from the [yourHooksGoHere.go](yourHooksGoHere.go) file, which is the
file that controls all of the plugins Slacker uses to interact with you and
your production environment.  Slacker comes with a variety of simple plugins to
get you started and give you examples to work from, and it's pretty easy to add
your own. [Making and managing your own plugins](docs/plugins.md) is pretty
much why you're here in the first place after all.

## Deploy Slacker to Heroku and be all #legit in 10 minutes

0: Have a github account, a Heroku account, Heroku Toolbelt installed, and upload your ssh key to Github and Heroku

1: Select *Configure Integrations* from your team menu in slack

2: Add a new *Bots* integration, give your bot a clever name, and take note of your Token

3: 
```
go get github.com/kr/godep
```

4: Go to https://github.com/djosephsen/slacker/fork to fork this repository (or click the fork button up there ^^) 

5 through like 27:  
```
mkdir -p $GOPATH/github.com/<yourgithubname>
cd $GOPATH/github.com/<yourgithubname>
git clone git@github.com:<yourgithubname>/slacker.git
cd slacker
git remote add upstream https://github.com/djosephsen/slacker.git
go get
godep save
heroku config:set SLACKER_NAME=<whatever you named your bot in the Slack UI>
heroku config:set SLACKER_TOKEN=<your token>
heroku config:set SLACKER_LOG_LEVEL=DEBUG
heroku create -b https://github.com/kr/heroku-buildpack-go.git
git push heroku master
```

At this point you should see slacker join your channel.

![hi](docs/screenshots/hi.png)

When you make changes or add plugins in the future, you can push them to heroku with: 

```
godep save
git add --all .
git commit -am 'snarky commit message'
git push && get push heroku
```

## Words Words Words
[RE: Adding, removing, and creating plugins](docs/plugins.md)
