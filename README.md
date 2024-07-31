# caddy pprof

What do you need?
1. caddy running and access to its config (you will need to reload caddy)
2. a golang container locally (or wherever you decide to download the profile)
3. ability to ssh tunnel from your machine inside the caddy container

## Admin access to caddy

Caddy admin interface is on port 2019 by default, but only listens to localhost. If that is a container, you need to make it available on all container interfaces.

Edit the global block of your Caddyfile:

```
{
    ...
    admin 0.0.0.0:2019
    ...
}
```

Now restart / reload caddy.
Please note, this can be a security risk. You do not want your admin port exposed to internet! 

## SSH tunnel into your caddy container.

Get the internal IP of the container (e.g. 10.22.22.22)
`ssh <caddy_host> -L 127.0.0.1:2019:10.22.22.22:2019`

The pprof debug page can be accessed here:
http://localhost:2019/debug/pprof/

## Get a CPU profile

To get a cpu profile, go to: and check if 
go to http://localhost:2019/debug/pprof/profile?debug=1&seconds=60 

wait for it and download the file as `profile-something`

## Create the SVG graph

Grab your profile file and copy it into a golang container, then make a nice svg:

`go tool pprof -output=nicesvg.svg -web <path/to/>profile-something`

