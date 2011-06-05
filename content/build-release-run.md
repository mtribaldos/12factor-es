## V. Build, release, run
### Strict separation of build stage and run stage

The process of turning a [codebase](/codebase) into a running app passes through three stages:

* The *build stage* is a transform which converts a code repo into an executable bundle known as a *build*.  Using a version of the code at a commit specified by the deployment process, the build stage fetches and vendors [dependencies](/dependencies) and compiles binaries and assets.
* The *release stage* takes the build produced by the build stage and combines it with the deploy's current [config](/config).  The resulting *release* contains both the build and the config and is ready for immediate execution in the execution environment
* The *run stage* (also sometimes referenced as "at runtime") runs app against the current release.  This typically happens by fetching and expanding the build, setting up the environment with the [config](/config), and then launching one or more of the app [processes](/processes).

In a traditional server-based hosting environment, it's easy to muddle together these stages.  For example, a developer might create the fresh checkout and install dependencies (a build), but then tweak the code in-place on the disk of the production deploy (violation of stage separation).

**The twelve-factor app uses strict separation between the build, release, and run stages.**  It is impossible to make changes to the code at runtime, since there is no way to propagate those changes back to the build stage.  Config is accessible at runtime but not at build time, since config can change without triggering a build.

Builds must be initiated by a developer, as an essential part of the process of deploying new code.  Runtime execution, by contrast, can happen automatically in cases such as a server reboot, or a crashed process being restarted by the process manager.  Therefore, the run stage should be kept to as few moving parts as possible, since problems that prevent an app from running can cause it to break in the middle of the night when no developers are on hand.  The build stage can be more complex, since errors are always in the foreground for a developer who is driving the deploy.  A failed build must abort the release process and avoid any disruption of the running app.

#### Release = build + config

![Code becomes a build, which is combined with config to create a release.](/images/release.png)

A *release* is a combination of a build (an executable bundle generated in the [build stage](#)) and a [config](#), a set of environment variables to determine runtime behavior.

New builds always trigger new releases (since the build has been updated).  Config changes also trigger a new release (since the config has been updated).  A release requires restarting all running [processes](#) in order to bring all processes onto the new release.

Deployment tools typically offer release management tools, most notably the ability to roll back to a previous release.  For example, the [Capistrano](https://github.com/capistrano/capistrano/wiki) deployment tool stores releases in a subdirectory named `releases`, where the current release is a symlink to the current release directory; and offers a `rollback` command.

Every release should always have a unique release ID, such as a timestamp of the release (such as `2011-04-06-20:32:17`) or an incrementing number (such as `v100`).  Releases are an append-only ledger and a release cannot be mutated once it is created.  Any changes must create a new release.
