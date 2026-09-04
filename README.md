# Discourse stack for Kubernetes on Wodby

Deploy [Discourse](https://www.discourse.org/) applications on Kubernetes with Wodby.

The stack uses a connected source build rather than a prebuilt Wodby Discourse image. Select the upstream stable or ESR boilerplate, clone one into your own repository, or connect a compatible Discourse fork. Wodby CI builds the selected source on the official `discourse/base` image.

## Services

- **Discourse** builds and runs nginx, Unicorn, and Sidekiq in one stateful pod.
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

Upgrade by rebuilding from a newer supported Discourse Git ref. Do not update Discourse from its administration interface.

This initial stack intentionally uses one Discourse replica. A future high-availability variant must separate web and Sidekiq workloads, coordinate migrations, and move uploads to shared or object storage before increasing replica counts.
