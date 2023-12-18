# UAAC

## Readonly Client

Create an opsman client with readonly permissions with `uaac`

```bash
uaac client add <client_name> --authorized-grant-types client_credentials --authorities opsman.restricted_view --secret <secret>
```

### Links
- <https://docs.vmware.com/en/VMware-Tanzu-Operations-Manager/3.0/vmware-tanzu-ops-manager/opsguide-config-rbac.html#roles-in-tanzu-operations-manager-0>
