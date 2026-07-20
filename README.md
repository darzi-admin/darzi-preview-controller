# Darzi preview deployment controller

This public repository controls approved deployments of the synthetic Darzi
Phase 0 preview. The application source remains private in
`darzi-admin/darzi-v1`.

## Security boundary

- No application source, compiled application, client data, credentials, or
  Cloudflare identity lists may be committed here.
- `main` is the trusted controller branch and must remain protected.
- Deployments must use the `preview` environment with an independent required
  reviewer and prevent-self-review enabled.
- Environment secrets are released only after approval.
- The controller accepts only the current private integration SHA, reverifies
  it, deploys only to `darzi-v1-preview`, and rejects any production deployment.
- Cloudflare Access must require MFA for the exact approved identities.

The initial commit is the recorded one-time empty-repository bootstrap. The
reviewed deployment workflow remains fail-closed until an independent reviewer,
all least-privilege environment secrets, and the exact Cloudflare Access policy
are configured.

## Required environment secrets

- `SOURCE_REPO_TOKEN`: fine-grained, read-only access to contents and Actions
  in `darzi-admin/darzi-v1` only.
- `CLOUDFLARE_ACCOUNT_ID`
- `CLOUDFLARE_PAGES_API_TOKEN`: Pages Write only.
- `CLOUDFLARE_ACCESS_API_TOKEN`: Access Apps and Policies Read only.
- `CLOUDFLARE_ACCESS_ALLOWED_EMAILS_JSON`
- `CLOUDFLARE_ACCESS_ALLOWED_GROUP_IDS_JSON`

Never place secret values in issues, pull requests, workflow inputs, logs, or
repository files.
