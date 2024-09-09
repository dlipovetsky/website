---
title: "Observations on the Kubernetes Operator pattern"
date: 2020-10-11T19:56:50Z
type: "post"
---

## Applications Evolve

As applications evolve, sometimes the new version becomes incompatible with
the old. In that case, upgrading from old to new requires a migration. The
migration can be complicated, and can require you to understand the
application at the level of a maintainer, rather than a user. And once you've
understood how to perform the migration, you need to understand how to
automate it, how to recognize when the migration fails, and to safely
rollback.

For an application that runs in a Kubernetes cluster, automated migration can
be the domain of an Operator, something that manages that application's
lifecycle.

## Operators Enable Evolution

The alternative to migration is making all evolution backward-compatible. For
argument's sake, let's agree that there are valid and good reasons to make
breaking changes, and therefore to require migration. Migration is typically
not the domain of the application itself. Migration might vary depending on
the environment in which the application runs, and it is not the
application's job to understand all of these environments. But, because the
application doesn't handle migration, end-users must cobble it together based
on documentation.

An Operator can handle migration. Its job is made easier because it doesn't
need to handle arbitary environments, but can expect certain behaviors from
Kubernetes. Still, it has to support some variation in environments.

Individual end-users don't need to put in as much work to migrate, have less
reason to fear and loathe migration, and maintainers have more (though not
unlimited) freedom to evolve the application.

## Operators Enable Collaboration

Automating migration is no easy task. From 2016 to 2019, I worked on a
service that automated "cluster lifecycle" for Kubernetes. During that time,
the single stateful component of the Kubernetes control plane, the etcd
database, went through a major version change (v2 to v3). In the standard
case, migration was hard, but became an order of magnitude harder in some
edge cases.

My coworkers and I spent a lot of effort to automate migration. We were not
the only ones; other engineers, working on similar services, were solving the
same problems. In hindsight, we would all have benefitted from collaborating
on these problems, but because we were implementing for different services,
collaboration was impractical, if not impossible.

With Kubernetes as the common service, the Operator can be the point of
collaboration. End-users can share in the work of understanding and
automating migration, and other application lifecycle problems.
