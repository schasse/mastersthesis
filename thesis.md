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
