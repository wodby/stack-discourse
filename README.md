# Wodby stack template

This repository is a starter for a Git-backed Wodby stack. Wodby imports
`stack.yml` from the selected Git ref and creates a new stack revision every
time you import or update it from Git.

The included manifest defines a small stack with one required nginx service.
Use it as a working baseline, then replace the service references and overrides
with the services your stack should manage.

## Files

- `stack.yml` - the Wodby stack manifest.

## Start here

1. Change `name`, `title`, and `icon` in `stack.yml`.
2. Replace `services[].service` with the Wodby service name or versioned service
   reference you want to include, for example `php` or `php:8.3`.
3. Keep `services[].name` as the stack-local service name. It does not have to
   match the referenced service name.
4. Use `services[].links` to satisfy required links declared by service
   manifests.
5. Use `services[].workloads`, `services[].env`, `services[].volumes`,
   `services[].configs`, and `services[].helm` only for stack-level overrides.

If you use this with the companion service template, import the service first
and then change the stack service reference to `example-service:1.0`.

## Multiple stacks

A repository can contain multiple stacks. Put each stack in its own directory
and add an `index.yml` at the repository root:

```yaml
stacks:
- api
- worker
```

Each listed directory must contain its own `stack.yml`.

## References

- Stack template reference: https://wodby.com/docs/2.0/stacks/template/
- Stack services: https://wodby.com/docs/2.0/stacks/services/
- Naming rules: https://wodby.com/docs/2.0/naming/
