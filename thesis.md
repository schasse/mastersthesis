# from blue green deployments to phoenix to rolling updates. to canary releasing, to rolling updates

The approach of blue green deployment involves two environments. One
is the environment which serves production traffic, the other
environment is in standby. To the other environment you deploy the new
version. Then you switch routing from the environment running the old
version, to the environment running the new version.

This has the major disadvantage that it wastes resources. Just one
environment is serving production traffic as the other is just
idling. Even if you use the idling environment as a staging
environment it's oversized and still wasting resources.

In times of dynamic resource allocation, automation and infrastructure
as code, it's easy to create a new environment or at least the needed
servers. After the switching of the traffic is done you can throw away
the old version. This procedure is called phoenix deployment.


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

# rollbacks
