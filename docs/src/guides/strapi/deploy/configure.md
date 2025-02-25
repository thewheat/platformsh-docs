---
title: "Configure Strapi for Platform.sh"
sidebarTitle: "Configure"
weight: -100
toc: false
description: |
  Review the basics of what makes up a Platform.sh project, including its three principle configuration files and how to define them for Strapi.
---

{{< guides/config-desc name="Strapi" >}}

## Requests configuration: `routes.yaml`

{{< guides/config-routes template="strapi4" name="Strapi" >}}

## Service configuration: `services.yaml`

{{% guides/config-service framework=Strapi %}}

Strapi requires a database to be able to deploy.
By default, it uses a SQLite database but other database types are also supported.
These other database types are Oracle MySQL, PostgreSQL, or MongoDB (available only in Strapi v3 and below).
The Strapi template defines a PostgreSQL database service.
To use another service, replace `postgresql:12` in the example below with the name and version of the database you want.

{{% /guides/config-service %}}

```yaml
# The services of the project.

# Each service listed will be deployed
# to power your Platform.sh project.

db:
  type: postgresql:12
  disk: 256

# Uncomment the line below to use a MySQL database
# dbmysql:
#   type: oracle-mysql:8.0
#   disk: 256
```

## Application container: `.platform.app.yaml`

{{< guides/config-app template="strapi4" >}}

In the Strapi template, `yarn` is run during the build hook to install all of Strapi’s dependencies, and then `yarn build` is run to build the site.
If you would rather use npm to manage your dependencies, you can:

1. Delete `yarn` from the build hook.
2. Replace `yarn build` in the build hook with `npm run build`.
3. Delete the `build.flavor` block.
   When this is set to `none`, Platform.sh to rely solely on the build hook to define the build process.
   By default, Node.js containers run `npm install` prior to the build hook,
   so this block can be removed entirely from the configuration.
4. Delete the `dependencies` block, which includes yarn.

The relationships block is responsible for providing access to the data sources (services) that the Strapi application needs.

Since Platform.sh is read-only during build, mainly for security purposes, certain folders need to be mounted.
Platform.sh allows you to mount directories that need write access during the deploy phase with the `mounts` key.
In this case, the following folders are mounted for Strapi.

- `.cache` file
- `.tmp` file
- `database` folder
- `extensions` folder
- `uploads` folder in the `public` directory

{{< /guides/config-app >}}

{{< guide-buttons next="Deploy Strapi" >}}
