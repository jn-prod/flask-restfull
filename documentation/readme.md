![docker logo](images/docker-logo.jpg)

### Fitter. Happier. More Productive.

<hr>

#### - Docker in Action -

Note:
- play radiohead song
- Alright, we have a lot of material to cover so let's dive right in.

---

## Joke

<hr><br>

### What did the hippie/yogi say when asked to get off the couch?

----

Namaste<br>(nah-I'm-a-stay)

---

## Workflow!

<hr><br>

![steps](images/steps.jpg)

Note:
1. Code locally on a feature branch
1. Open a pull request on Github against the master branch
1. Run automated tests against the Docker container
1. If the tests pass, manually merge the pull request into master
1. Once merged, the automated tests run again
1. If the second round of tests pass, a build is created on Docker Hub
1. Once the build is created, it's then automatically (err, automagically) deployed to production

---

# Part 1 - Local Setup

----

## Docker Setup

<hr>

Let's get Docker up and running!

<br>

Enter **Boot2Docker** - a *lightweigh*t Linux distribution designed specifically to run Docker ([Install](https://docs.docker.com/installation/mac/))

Note: Mac and Windows users need this in order to power Docker containers. Once installed, test it out - `boot2docker version`

----

## Fig Up!

<hr>

![steps](images/fig.png)

- **[Fig](http://www.fig.sh/)** builds and run services needed for your application.
- Install it via pip and then define the services within a *fig.yml* file.

Note:
- Fig handles the building and running of multiple services. It makes it super easy to link services together running in different containers.
- Once installed, test it out - `fig --version`
- First, we build the image from the "server" directory and then mount that directory to the "code" directory within the Docker container. The Flask app is ran via the `python app.py` command. This exposes port 5000 on the container, which is forwarded to port 80 on the host environment.
- Next, the Redis service is built from the Docker Hub "Redis". Port 6379 is exposed and fowarded.

----

## Build and Run

<hr><br>

### `$ fig up`

![fig logo](images/figup.png)

Note:
- This command builds an image for our Flask app, pulls the Redis image, and then starts everything up.
- Now we can view our app in the browser by navigating to the address associated with the boot2docker VM - `boot2docker ip`.(Run 'docker-machine ip dev' to get the address.)
- At this point we've set up our local environent. Next, let's add CI into the mix.

---

# Part 2 - Continuous Integration

----

## Docker Hub

<hr>

- [Docker Hub](https://hub.docker.com/) is a repository of Docker images.
- It acts as a CI server since you can configure it to create a build every time you push a new commit to Github.

Note:
- Docker Hub is like Github, only for Docker images
- CI: Let's test this out. I'll add a simple assertion to the test, commit, and push. This will generate a new build on Docker Hub - which should be successful.
- That said, we should use a *true* CI server to catch errors before we generate any builds on Docker Hub.

----

## CircleCI

<hr>

![circleci logo](images/circleci.png)

- **[CircleCI](https://circleci.com/)** is a continuous integration and delivery platform that supports testing within Docker containers.
- [Getting started with CircleCI](https://circleci.com/docs/getting-started) - super simple setup
- Add a *circle.yml* config file

Note:
- Given a Dockerfile, CircleCI builds an image, starts a new container, and then runs tests inside that container.
- Sign up with your Github account, then add the Github repo to create a new project. This will automatically add a webhook to the repo so that anytime you push to Github a new build is triggered.
- Essentially, we create a new image, run the container, then test - first that the web process is running (the app is live) and then that our unit tests pass.

----

## Feature Branch Workflow

<hr>

Example...

1. Create the branch
1. Make changes locally
1. Issue a Pull request
1. Manually merge once the tests pass
1. The tests run again on the master branch
1. Once the second round passes, a new build is triggered on Docker Hub

Note:
- Before you go through this process, you need to configure Docker Hub to only create a new build after Circle CI runs the tests on the master branch - and they are successful. This is detailed in the blog post.
- `git checkout -b circle-test master`
- `self.assertNotEqual(four, 6)`
- `git push origin circle-test`
- Create PR
- Merge
- Check for new build

---


## Conclusion

<hr>

- So, we went over a nice development workflow that included setting up a local environment coupled with continuous integration.
- What about the final piece - delivering this app to the production environment? Make sure to check out the blog post @ [realpython.com](https://realpython.com) on 02/10/2015!
- michael@realpython.com

![heart](images/heart.jpg)

---
