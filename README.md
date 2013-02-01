Drupal Fracture
===============

This is an attempt to break out all the core modules/themes/profiles or
Drupal into independantly versioned sub-projects. The goal is to do so
in such a way that git-subtree can be used to aggregate/disaggregate the
components/core regardless of where changes are commited.

The scripts will split out the components and add initial composer.json
(and composer.lock?) files. It might also tag the repo appropriately.

Goals
-----

- Demonstrate how Composer would work in a more distributed core.
- Demonstrate how semver can be applied better when separate components
are versioned like separate components.


Roadmap
-------

- Run the split scripts on cron daily, and push resultant repos to to
drupal-fracture github organization.
- Figure out a better way to detect when a module has been changes, so
we're not running subtree without changes.
- Move tests into component repos.
- Implement travis CI.
- Get drupal-github project authenticating with GitHub API, and try to
demonstrate how projects might be hosted on GitHub without disrupting
how the community works on d.o:
http://github.com/patcon/drupalgithub
- Upload components to Packagist.
