# Darzi preview deployment controller

This public repository controls approved deployments of the synthetic Darzi
Phase 0 preview. The application source remains private in
`darzi-admin/darzi-v1`.

## Security boundary

- No application source, compiled application, client data, credentials, or
  Cloudflare identity lists may be committed here.
- `main` is the trusted controller branch and must remain protected.
- Deployments must use the `preview` environment from protected `main` after an
  explicit owner dispatch with the exact tested source SHA and confirmation.
- The source build job receives only a read-only private-source token and never
  receives Cloudflare credentials.
- Cloudflare environment secrets are available only to a fresh environment
  runner that downloads the exact digest-bound static artifact. It never
  executes private-source tooling.
- The controller accepts only the current private integration SHA, reverifies
  and builds it, deploys only to `darzi-v1-preview`, and rejects any production
  deployment.
- Cloudflare Access must require MFA for the exact approved identities.

The initial commit is the recorded one-time empty-repository bootstrap. Because
Darzi currently has one GitHub operator, Phase 0 uses a recorded owner-only
approval exception. This reduces protection against compromise of that account;
the required status check, pull-request history, exact-SHA verification,
main-only environment policy and no-admin-bypass setting remain mandatory. The
workflow remains fail-closed until all least-privilege secrets and the exact
Cloudflare Access policy are configured.

## Required repository secret

- `SOURCE_REPO_TOKEN`: fine-grained, read-only access to contents and Actions
  in `darzi-admin/darzi-v1` only.

## Required `preview` environment secrets

- `CLOUDFLARE_ACCOUNT_ID`
- `CLOUDFLARE_PAGES_API_TOKEN`: Pages Write only.
- `CLOUDFLARE_ACCESS_API_TOKEN`: Access Apps and Policies Read only.
- `CLOUDFLARE_ACCESS_ALLOWED_EMAILS_JSON`
- `CLOUDFLARE_ACCESS_ALLOWED_GROUP_IDS_JSON`

Never place secret values in issues, pull requests, workflow inputs, logs, or
repository files.
