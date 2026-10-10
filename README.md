# AWS bootstrap

These runbooks take a new AWS customer from one account to a governed organization with separate dev and prod workload accounts. They use AWS Organizations, Control Tower, and IAM Identity Center, and they leave no long-lived AWS credentials behind. Projects then bootstrap their own state, CI roles, and deployment access inside the workload accounts.

Follow the files in order:

1. [Plan accounts, names, email, and Regions](00-plan-account-layout.md)
2. [Create and secure the management account](10-secure-management-account.md)
3. [Create the Control Tower landing zone](20-create-control-tower-landing-zone.md)
4. [Configure initial Production controls](25-configure-production-controls.md)
5. [Configure Identity Center access](30-configure-identity-center-access.md)
6. [Provision stable and disposable accounts](40-provision-workload-accounts.md)
7. [Centralize member-account root access](50-centralize-member-root-access.md)
8. [Configure local SSO profiles](60-configure-local-sso.md)
9. [Operate and retire accounts](90-operate-account-lifecycle.md)

If Control Tower and IAM Identity Center are already healthy and the only human access is an administrator in the management account, start at step 5.

The result looks like this:

```text
AWS organization
├── management account             organization administration only
├── Security OU
│   ├── Log Archive account        Control Tower managed
│   └── Audit account              Control Tower managed
├── Sandbox OU
│   ├── dev account                shared development; CI deploys here
│   └── dev-<owner> accounts       disposable; deployed locally
└── Production OU
    └── prod account               production; CI deploys here
```

Do not put root passwords, MFA recovery material, AWS access keys, account request files, account IDs, or populated infrastructure inputs in Git.
