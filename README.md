# external-secrets Vault token renewer [esvtr]

Kube CronJob to renew tokens used by external-secrets

The CronJob needs a token that allows the renew via accessor passed via secret (can obviously be created using external-secrets)

```
path 'auth/token/renew-accessor' {
  capabilities = ["update"]
}
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: esvtr-master-token
  namespace: external-secrets
stringData:
  token: hvs...
```

It then needs two config maps, the first containing the list of accessors to be renewed and the second with the settings for making the requests

###### This cron use healthchecks.io to monitor the cron status

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: esvtr-accessor-list
  namespace: external-secrets
data:
  accessor.lst: |-
    bF...
    YY...
    52...
    RR...

---

apiVersion: v1
kind: ConfigMap
metadata:
  name: esvtr-config
  namespace: external-secrets
data:
  VAULT_URL: vault url
  HC_URL: healthchecks url
  HC_UUID: healthcheks uuid
```

