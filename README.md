# Ansible Role Deployment

This role is designed to handle the generic aspects of an application
deployment, roughly: clone, perform file and database operations, build,
put into production, and cleanup files.

Specifically, it performs the following tasks:

- `setup.yml`
  Collect information (timestamp) about the current deployment and the last
  deployment (file and directory names).
- `files.yml`
  - Create a new directory with a name like `example.com-2017-05-29T08-03-22Z`.
  - Clone application repository into the new directory.
  - Create an arbitrary (and configurable) set of files, directories,
    and symlinks.
- `mysql.yml`
  - Ensure database is present.
  - Ensure database user with appropriate access exists.
  - Create database dump with a name like `example_db-2017-05-29T08-03-22Z.sql`
    in the _last_ deployment's directory.
- `{{ deployment_application_role }}`
  - Run a configurable role containing the tasks specific to your
    application.
- Switch the existing deployment symlink from the _last_ deployment
  directory to the _current_ deployment directory.
- `cleanup.yml`
  - Create a tarball of the previous deployment directory.
  - Remove the previous deployment directory.
  - Remove all but the last (configurable) _n_ deployment tarballs.

The directory structure in which all this happens is configurable, but
the following works well:

```
/
└── var
    └── www
        └── example
            ├── deployments
            │   └── example.com-2017-05-30T04-26-47Z
            │       └── example.com
            │           └── docroot
            └── example.com -> /var/www/example/deployments/example.com-2017-05-30T04-26-47Z/example.com
```

In this scheme:
  - `/var/www/example` is the client directory.
  - `/var/www/example/deployments` is where all application files live.
  - `/var/www/example/deployments/example.com-2017-05-30T040-26-47Z` is
    the _current_ deployment directory.
  - `/var/www/example/deployments/example.com-2017-05-30T040-26-47Z/example.com`
    is the newly cloned git repository.
  - `/var/www/example/example.com` is a _symlink_ to the current
    directory.
  - The virtual host directory would be `/var/www/example/example.com/docroot`.

## Requirements

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

## Role Variables

@todo

## Example Playbook

@todo

## License

MIT

## Author Information

Christopher Torgalson / manager@bedlamhotel.com
