# Discourse stack for Kubernetes on Wodby

Deploy [Discourse](https://www.discourse.org/) applications on Kubernetes with Wodby.

The stack runs the prebuilt [wodby/discourse](https://github.com/wodby/discourse) image, with no repository and no build. Choose the monthly release (`2026.10`) or the extended support release (`2026.7`). Plugins bundled with Discourse are available, and themes are installed from the administration interface; other plugins are not supported.

## Services

- **Discourse** runs nginx, Unicorn, and Sidekiq in one stateful pod.
- **PostgreSQL** stores forum data and provisions `hstore`, `pg_trgm`, `unaccent`, and `vector`.
- **Redis** stores cache, message-bus, and Sidekiq state with persistence enabled and eviction disabled.
- **OpenSMTPD** delivers forum email directly or through a selected third-party SMTP integration.

All four services are required because working outbound email is part of Discourse account and administration workflows.

## Initial configuration

Set `Administrator email addresses` on the Discourse service before the first deployment. For reliable delivery, attach an SMTP integration to OpenSMTPD.

## Capacity

The default storage allocation is:

- 20 GiB for Discourse uploads, backups, and logs
- 20 GiB for PostgreSQL
- 5 GiB for Redis persistence
- 1 GiB for the OpenSMTPD queue

The Discourse container requests 2 GiB of memory and 0.5 CPU, with limits of 4 GiB and 2 CPU.

## Backups and upgrades

Use the Discourse service backup for a coherent archive containing the database and local uploads. Regularly restore an artifact into a disposable app to verify disaster recovery.

Discourse is upgraded by a newer image: a new revision of the selected release line brings its patch releases. Do not update Discourse from its administration interface.

Applications created from earlier versions of this stack build Discourse from a repository and keep doing so. To move one to the prebuilt image, create a new application from this stack and restore a Discourse backup into it.

This initial stack intentionally uses one Discourse replica. A future high-availability variant must separate web and Sidekiq workloads, coordinate migrations, and move uploads to shared or object storage before increasing replica counts.

<!-- wodby:generated:start -->

## Stack contract

- [Discourse stack on Wodby](https://wodby.com/stacks/discourse)
- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Discourse stack guide](https://wodby.com/docs/2.0/stacks/catalog/discourse/)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [Discourse service](https://github.com/wodby/service-discourse)
- [PostgreSQL service](https://github.com/wodby/service-postgres)
- [Redis service](https://github.com/wodby/service-redis)
- [OpenSMTPD service](https://github.com/wodby/service-opensmtpd)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Discourse<br>`discourse` | required; enabled by default; volumes: `data` 20 GB; links: `db` → `postgres`, `redis` → `redis`, `sendmail` → `opensmtpd` |
| PostgreSQL<br>`postgres` | required; enabled by default; volumes: `data` 20 GB |
| Redis<br>`redis` | required; enabled by default; volumes: `data` 5 GB |
| OpenSMTPD<br>`opensmtpd` | required; enabled by default; volumes: `spool` 1 GB |

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Validate the stack manifest

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

<!-- wodby:generated:end -->
