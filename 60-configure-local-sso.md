# Configure local SSO profiles

Install AWS CLI v2. Never run `aws configure` with access-key credentials.

Create one profile per account and permission set. Pick a short prefix for the organization or project; this guide uses `<prefix>`:

```console
aws configure sso --profile <prefix>-management-admin
aws configure sso --profile <prefix>-dev-admin
aws configure sso --profile <prefix>-prod-admin
aws configure sso --profile <prefix>-prod-readonly
```

For each profile:

- Use the same SSO session name and AWS access portal URL.
- Set **SSO Region** to the Identity Center/Control Tower home Region.
- Select the matching account and permission set.
- Set **Default client Region** to your workload Region.
- Keep `dev` or `prod` in workload profile names. Project tooling can use the name as an account safety signal.

Use the administrator profiles for bootstrap and break-glass work only. When a project grants a narrower dev permission set, add an everyday profile such as `<prefix>-dev` for it. Use `<prefix>-prod-readonly` for routine production inspection.

For a disposable account, use a name such as `<prefix>-dev-<owner>-admin`.

Verify one profile at a time:

```console
aws sso login --profile <prefix>-dev-admin
aws sts get-caller-identity --profile <prefix>-dev-admin
```

Compare the returned account ID with the access portal before applying anything. The AWS CLI stores SSO configuration and temporary cached tokens under `~/.aws`. Do not copy `~/.aws`, cached tokens, or shell exports into Git.

Reference: [Configure IAM Identity Center authentication for AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html).

Next: [Operate and retire accounts](90-operate-account-lifecycle.md).
