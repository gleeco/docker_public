# Instant RelateIQ Development Environment

Read through [A Docker Dev Environment in 24 Hours! (Part 2 of 2)](http://blog.relateiq.com/a-docker-dev-environment-in-24-hours-part-2-of-2/) then come back here.

## Installation:

### Linux

Install Virtualbox based off the [installation instructions](https://www.virtualbox.org/wiki/Linux_Downloads).

### MacOS

#### Install Homebrew

First, install [Homebrew](http://brew.sh/).

```
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

#### Install Virtualbox and Vagrant

Install VirtualBox and Vagrant using [Brew Cask](https://github.com/phinze/homebrew-cask).

```
brew tap phinze/homebrew-cask
brew install brew-cask
brew cask install virtualbox
brew cask install vagrant
```

## Check out the repository

```
git clone https://github.com/relateiq/docker_public
cd docker_public
vagrant up
```

After Guest Additions is in, reload the vagrant:

```
vagrant reload
vagrant ssh
```

## Update devenv

This will pull all the images from the server and update Docker's configuration for Shipyard.

```
./bin/devenv update
vagrant reload
```
## Run devenv start

Start the devenv container:

```
./bin/devenv start
```

You should see:

```
➜  docker_public git:(master) ✗ ./bin/devenv start
Started ZOOKEEPER in container b7af4a32fd11
Started REDIS in container f49c9910da35
Started CASSANDRA in container 52f3233972fb
Started ELASTICSEARCH in container 802a9f1139d9
Started MONGO in container 33e5df9178a4
Started KAFKA in container d3475cfe8161
```

### Configure Shipyard UI

Once the server has rebooted and devenv started, you should have shipyard up --running in a container

First, we need to fetch & run a 3rd-party agent. (TODO --automate)

* `curl https://github.com/shipyard/shipyard-agent/releases/download/v.0.0.9/shipyard-agent -L -o bin/shipyard-agent`
* `chmod 755 bin/shipyard-agent`
* NB: check the latest version before you download
* NB: check the port shipyard is running on (inside the VM)
* SSH inside the VM and run `/vagrant/bin/shipyard-agent -url http://localhost:8005 -register`
* this prints a key to stdout which you then use:
    `/vagrant/bin/shipyard-agent -url http://localhost:8005 -key <KEY>`

Now back to the shipyard UI:

* Log in at http://localhost:8005/ with admin/shipyard as creds
* Go to http://localhost:8005/hosts/ to see Shipyard's hosts.
* In the vagrant VM, `ifconfig eth0` to verify your VM instance IP addr.


Now look at [http://localhost:8005/containers/](http://localhost:8005/containers/) and you should see your containers.

![Containers](shipyard.png)

## Conclusion


You're all done!

