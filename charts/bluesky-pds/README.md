# bluesky-pds

A Helm chart to deploy a [Bluesky PDS](https://github.com/bluesky-social/pds) on Kubernetes.

![Version: 0.1.2](https://img.shields.io/badge/Version-0.1.2-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.4.59](https://img.shields.io/badge/AppVersion-0.4.59-informational?style=flat-square)

## Installing the Chart

```bash
helm repo add lightcube https://helm.e99.at
helm repo update
helm install lightcube/bluesky-pds bluesky-pds -f values.yaml
```

## Bluesky PDS configuration and account creation

Regarding configuration and account creation, refer to the [official PDS docs](https://github.com/bluesky-social/pds/blob/main/README.md).

### Generate invite code

```bash
export ADMINPW=your-admin-pw
export PDS_HOSTNAME=pds.example.com
curl --silent --show-error --request POST --header "Content-Type: application/json" "$@" \
    --user "admin:${ADMINPW}" \
    --data '{"useCount": 1}' \
    "https://pds.example.com/xrpc/com.atproto.server.createInviteCode" | jq --raw-output '.code'
```

### Create account

* Create the json input

```json
{
  "email": "user@example.com",
  "handle": "user.pds.example.com",
  "password": "my-password",
  "inviteCode": "invite-code"
}

```

* Create the account

```bash
curl --silent --show-error --request POST --header "Content-Type: application/json" -d @data.json "https://${PDS_HOSTNAME}/xrpc/com.atproto.server.createAccount"
```

Once this is done, you should be able to login on https://bsky.app/ using your PDS.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"ghcr.io/bluesky-social/pds"` |  |
| image.tag | string | `"0.4.59"` |  |
| imagePullSecrets | list | `[]` |  |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `""` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"pds.example.com"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"Prefix"` |  |
| ingress.tls | list | `[]` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| pds.config.blobstoreLocation | string | `"/pds/blocks"` |  |
| pds.config.bskyAppViewDid | string | `"did:web:api.bsky.app"` |  |
| pds.config.bskyAppViewUrl | string | `"https://api.bsky.app"` |  |
| pds.config.crawlers | string | `"https://bsky.network"` |  |
| pds.config.dataDir | string | `"/pds"` |  |
| pds.config.didPlcUrl | string | `"https://plc.directory"` |  |
| pds.config.hostname | string | `"pds.example.com"` |  |
| pds.config.reportSvcDid | string | `"did:plc:ar7c4by46qjdydhdevvrndac"` |  |
| pds.config.reportSvcUrl | string | `"https://mod.bsky.app"` |  |
| pds.config.secrets.adminPassword | string | `""` |  |
| pds.config.secrets.jwtSecret | string | `""` |  |
| pds.config.secrets.plcRotationKey | string | `""` |  |
| pds.dataStorage.csiAttributes | object | `{}` |  |
| pds.dataStorage.csiDriver | string | `nil` |  |
| pds.dataStorage.mountPath | string | `"/pds"` |  |
| pds.dataStorage.size | string | `"10Gi"` |  |
| pds.dataStorage.storageClass | string | `nil` |  |
| pds.dataStorage.volumeHandle | string | `nil` |  |
| podAnnotations | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| replicaCount | int | `1` |  |
| resources | object | `{}` |  |
| securityContext | object | `{}` |  |
| service.port | int | `3000` |  |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.annotations | object | `{}` |  |
| serviceAccount.create | bool | `true` |  |
| serviceAccount.name | string | `""` |  |
| tolerations | list | `[]` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)