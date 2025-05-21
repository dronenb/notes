# Cloud Foundry

Notes for things related to Cloud Foundry. In particular, Tanzu Application Service, VMWare's offering of Cloud Foundry.

## CredHub

### Hijack CredHub Credentials from running app (assumes Java buildpack)

```bash
cat /proc/$(ps -ef | grep -v grep | grep java | awk '{print $2}'|head -n 1)/environ|tr '\\0' '\\n'|grep VCAP_SERVICES|cut -d '=' -f 2-|jq '.credhub[] | select (.instance_name=="credhubSi")|.credentials' -c -r | jq -r .
```

### Get credhub creds when using `cf ssh`

```bash
curl -H 'Content-Type: application/json' --cert /etc/cf-instance-credentials/instance.crt --key /etc/cf-instance-credentials/instance.key -d "$VCAP_SERVICES" 'https://credhub.service.cf.internal:8844/api/v1/interpolate'
```

### Use foundation CredHub CLI

```bash
credhub api -s $BOSH_ENVIRONMENT:8844 --ca-cert $BOSH_CA_CERT
export CREDHUB_CLIENT=$BOSH_CLIENT
export CREDHUB_SECRET=$BOSH_CLIENT_SECRET
export CREDHUB_SERVER=$BOSH_ENVIRONMENT:8844
export CREDHUB_CA_CERT=$BOSH_CA_CERT
```

### Check maestro cert when a violation occurs when regenerating CA

```bash
maestro topology --name
```

## Diego

### Get running apps in Diego cell

```bash
cfdot cell-state <cell-id> | jq -r '. | "\ndiego_cell:", .cell_id, "\n", "App-Guids:", .LRPs[].process_guid[0:36]'
```

Sometimes this doesn't work, in which case, see the second link below

- <https://community.pivotal.io/s/article/How-to-find-which-Apps-are-Running-in-a-Diego-Cell?language=en_US)>
- <https://community.pivotal.io/s/article/Alternate-method-to-locate-an-app-instance-guid?language=en_US>

## `uaac`

### Login with client credentials

```bash
uaac target "${OM_TARGET}/uaa"
uaac token client get "${OM_CLIENT_ID}" -s "${OM_CLIENT_SECRET}"
```

### Opsman Client Creation

Create an opsman client with readonly permissions with `uaac`

```bash
uaac client add <client_name> --authorized-grant-types client_credentials --authorities opsman.restricted_view --secret <secret>
```

- <https://docs.vmware.com/en/VMware-Tanzu-Operations-Manager/3.0/vmware-tanzu-ops-manager/opsguide-config-rbac.html#roles-in-tanzu-operations-manager-0>
