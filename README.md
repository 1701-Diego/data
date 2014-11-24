# Hackathon Resources

## Diego API Docs

Diego provides a RESTful API for running Tasks and Long Running Processes.  Here's an outline of the API docs (all subject to change of course):

- [API Overview](overview.md)
- [Understanding Tasks](tasks.md)
- [Understanding Long Running Processes (LRPs)](lrps.md)
- [Container Runtime Environment](environment.md)
- [Available Actions](actions.md)
- API Reference
    - [Tasks](api_tasks.md)
    - [LRPs](api_lrps.md)
    - [Cells](api_cells.md)

We recommend using the API client provided by Diego's Receptor here:

https://github.com/cloudfoundry-incubator/receptor

[laforge](#laforge) has some example usages.

## Hackathon Resources

We've built a bunch of handy utilities and examples for the Hackathon.  To use these set up the following environment variables

For Ketchup:

```
export RECEPTOR=http://username:password@receptor.ketchup.cf-app.com
export DOPPLER=wss://doppler.ketchup.cf-app.com:4443
```

we'll be providing the `username` and `password` at stand-up.

For Diego-Edge:

```
export RECEPTOR=http://receptor.192.168.11.11.xip.io
export DOPPLER=ws://doppler.192.168.11.11.xip.io
```

### Hackathon Rules

Diego doesn't have a multi-tenant API (CC provides that) so:

- Pick a `domain` for your team and use only that `domain`
- Don't mess with other people's Tasks and LRPs
- Be sure to specify your docker image correctly:
  `docker:///onsi/away-team` -- note the *three* leading slashes.
  
  Diego has an irritating bug when you don't specify `docker:///` and we may not fix it by the hackathon.
- Try things on Diego-Edge first.
- Be kind :)

### Diego-Edge

Diego-Edge is a lightweight packaged up version of Diego that you can deploy locally.  We recommend using Diego-Edge to tinker and explore before pushing work up to Ketchup.

To run Diego-Edge:

```
git clone http://github.com/pivotal-cf-experimental/diego-edge
cd diego-edge
vagrant up --provider=virtualbox
```

You can then use the Diego-Edge CLI:

```
go get github.com/pivotal-cf-experimental/diego-edge-cli
diego-edge-cli help
```

Or any of the various tools outlined below.

### Tools to interact with Diego

These assume you have [go](http://golang.org/doc/install#osx) installed

#### [`picard`](https://github.com/1701-diego/picard)

`picard` streams logs

```
go get github.com/1701-diego/picard
picard LOG_GUID
```

#### [`laforge`](https://github.com/1701-diego/laforge)

`laforge` can run a number of engineering experiments.  Feel free to build on top of `laforge` to add your own experiments.

```
go get github.com/1701-diego/laforge
laforge help
```

#### [`troy`](https://github.com/1701-diego/troy)

`troy` can fetch and display information about your Tasks and LRPs. 

```
go get github.com/1701-diego/troy
troy DOMAIN
```

#### [`worf`](https://github.com/1701-diego/worf)

`worf` can delete Tasks and LRPs

```
go get github.com/1701-diego/worf
worf delete-task TASK_GUID
worf delete-lrp LRP_GUID
```

### Things that run in Containers

Diego runs things.  Here are some things to run:

#### [`riker`](https://github.com/1701-diego/riker)

`riker` is a simple web app that tells you about its environment.

You can see examples of deploying `riker` in `laforge`.

You can fetch a linux binary from:

`http://onsi-public.s3.amazonaws.com/riker.tar.gz`

#### [`crusher`](https://github.com/1701-diego/crusher)

`crusher` is a health monitor.

You can see examples of deploying and using `crusher` in `laforge`.

You can fetch a linux binary from:

`http://onsi-public.s3.amazonaws.com/crusher.tar.gz`

`crusher` can either `--check-port=8080` or `--check-url=http://127.0.0.1:8080`

#### [`away-team`](https//github.com/1701-diego/away-team)

`away-team` is a Busybox-based docker image that packages up `riker` and `crusher`.  See `laforge` for example usage.
