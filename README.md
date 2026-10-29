# role-integrator

**Version:** 0.1.0 · `rasa.module.role.integrator` · private

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

- **As an agent's brain:** `galt agent new <name> --brain RasaOS/role-integrator@v0.1.0`. galt reads `role.json` and `prompt.md`. There is no role assignment this way, so no grants.
- **As the source of the published role:** the tenant's `roles/integrator.md` becomes the one line `element: RasaOS/role-integrator@v0.1.0`. After a person runs Publish roles, every agent with role `integrator` loads this Element. This needs galt's scheduler to advertise `role_elements` (galt ADR 0064).
- **Private:** a computer can fetch this repo only when the galt vault holds `RASAOS_GITHUB_TOKEN`. Without it the load fails closed.

## Changing the role

Until the tenant's `roles/integrator.md` points here, that file is what Publish roles reads, and this repo is a snapshot of it. Change both together; see `CLAUDE.md`.
