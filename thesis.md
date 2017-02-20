# From Blue-Green Deployments to Phoenix Replacement

The approach of blue green deployment involves two environments. One
is the environment which serves production traffic, the other
environment is in standby. You deploy to the other environment the new
version. You then switch routing from the environment, which runs the old
version, to the environment with the new version.

This has the major disadvantage that it is a waste of resources. Just
one environment doing work with serving production traffic as the
other environment is just idling. Even if you use the idling
environment as a staging it would be oversized and still wasting
resources.

In times of dynamic resource allocation, automation and virtual
machines, it became easy to automatically spawn new servers. In for a
deploy of a new version you would not change the servers, but
automatically create new ones with the version to deploy, then switch
the traffic from the old servers to the new servers, and in the end
just destroy the old servers. This procedure is called phoenix
replacement.


## Canary Releases

Canary releasing is a way to test new versions of the application in
production. But testing in production is risky, because when there is
an error which leads to a defect or failure the users will get
affected. And this costs money in any way.

There comes canary releasing into play. A single canary can't do much
harm and it's not a big catastrophe if the small canary is
malicious. Let me explain it at the example of a typically scaled web
application. Usually you do not have just a single web server, but
multiple servers. So when releasing in the canary style, you change
just a small proportion of the twenty webservers to the new
version. Now two versions of the webservers are running at the same
time.

Usually you want exactly the same version of webservers in production
with the exact same configuration. This makes systems easier to debug
in case of an error. The maximum count of different versions during
canary releasing is two. But why go for two different versions in
production with the canary releasing technique, when it makes
debugging in general harder.

It's because with canary releasing you can achieve different
goals. The first one is truly when an error occurs. Just because less
users are affected by the error. The majority of webservers is still
in the old version, so just a small portion of all the users have a
poor experience with the erroneous small canary version fraction.

In case of success, when everything works as expected, it is proven
that there is less risk of errors, even under real production
conditions.

canary releasing with features

canary releasing when doing refactorings.

## Rolling Deployments

The extend to the canary releasing technique is a rolling update. It
unifies phoenix replacement and canary releasing and extends it to a
full deployment. You start with one server in the new version and
spawn it automatically. Now it becomes part of the whole pool of
servers and serves traffic as the server is ready. Now two different
versions are serving traffic, just like with a canary release. But
after the first step the deployment goes on and then destroys a server
instance in the old version as you would do in the phoenix
replacement. Then the procedure will be done multiple times until all
servers of the old version are replaced by the new version.

# rollbacks
