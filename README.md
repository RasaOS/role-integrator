# role-integrator

**Version:** 0.1.2 · `rasa.module.role.integrator` · public

The **Integrator** role (`integrator`) as a RasaOS role module. Merges reviewed, green pull requests and deploys main to the lower environments (dev, then stage). Never production, never a version tag.

| File | What it is |
|---|---|
| `role.json` | The role's fields (id, name, summary, version, consumers, knowledge, takes, kinds, grants). galt publishes them to `/tenant_config/roles/integrator`. |
| `prompt.md` | The role's prompt, byte for byte the body of `roles/integrator.md` in vsi-tenant. |
| `.claude-plugin/plugin.json` | The plugin manifest, which makes the repo loadable into a session with `--plugin-dir`. |
| `rasa.json` | The Element manifest: `kind: module`, with the role's id and version under `rasa`. |
| `bin/check-role` | The release gate. |

Role version **8**, from `roles/integrator.md` at vsi-tenant `fd0c872`.

**Grants.** When a person assigns the role **active**, `role.json` opens these gate rows. Loading the Element alone opens none:

  - `merge` · `*` · `vsi-web`
  - `merge` · `*` · `galt`
  - `merge` · `*` · `vsi-ios`
  - `deploy` · `dev` · `vsi-web`
  - `deploy` · `stage` · `vsi-web`
  - `rules` · `dev` · `vsi-web`
  - `rules` · `stage` · `vsi-web`

**Work.** It takes `merge`, `deploy-lower` through `galt work`, per the tenant's `roles/WORK.md`.

## Loading it

- **As an agent's brain:** `galt agent new <name> --brain RasaOS/role-integrator@v0.1.2`. galt reads `role.json` and `prompt.md`. There is no role assignment this way, so no grants.
- **As the source of the published role:** the tenant's `roles/integrator.md` becomes the one line `element: RasaOS/role-integrator@v0.1.2`. After a person runs Publish roles, every agent with role `integrator` loads this Element. galt's scheduler supports this; it advertises `role_elements` (galt ADR 0064).
- **Public:** any computer can fetch it. No vault token is needed.

## Changing the role

Until the tenant's `roles/integrator.md` points here, that file is what Publish roles reads, and this repo is a snapshot of it. vsi-tenant's `/role` skill (`bin/role-element`) keeps them together; see `CLAUDE.md`.
