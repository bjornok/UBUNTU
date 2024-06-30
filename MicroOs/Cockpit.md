# Using Cockpit on my system

```console
$ sudo transactional-update pkg install patterns-microos-cockpit cockpit-ws cockpit-tukit
```

See: https://www.reddit.com/r/openSUSE/comments/voyqhi/anybody_successfully_running_cockpit_on_a_newer/?rdt=53206

We may install it in a container - but some permissions need to be set.

> I tried the native way and enabled cockpit.socket. All of the components were in place (as best as I could tell reading through
> the service files) and the port was open, but no dice on loading the page. Great call on the
> alternate container though - worked like a charm. For anybody that runs into the issue,
> I pulled this container: https://quay.io/repository/cockpit/ws

