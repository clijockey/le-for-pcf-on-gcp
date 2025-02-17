# Letsencrypt for PCF on GCP
This is the source code for a Docker image that does the following:

1. Requests a new certificate from Letsencrypt
2. Applies it to the GCP load balancer
3. Configures the following tiles with the new certificate:
	* PAS (aka Elastic Runtime)
	* PKS
	* Harbor

### Input Variables needed
* `PROJECT` - GCP Project that the cert and load balancer reside in
* `GCP_CREDENTIALS` - GCP credentials in JSON, you should request a service account with DNS Admin credentials
* `DOMAIN` - The domain you want to use, the usual PCF subdomains will auto be added to certs
* `LE_EMAIL` - email address to issue the certificates to
* `PCF_OPSMGR` - URL for opsmanager in the format `https://opsman.xxx.yyy`
* `PCF_PASSWD` - ops manager password
* `GCP_HTTPS_PROXY` - GCP Proxy to update - run `gcloud compute target-https-proxies list` to get this information

#### Optional input variables
* `SKIP_PAS_CERT` - skip updating the PCF PAS certificate - defaults to `false`
* `SKIP_PKS_CERT` - skip updating the PKS certificate - defaults to `false`
* `SKIP_HARBOR_CERT` - skip updating the VMware Harbor certificate - defaults to `false`
* `SKIP_OPSMAN_APPLY` - skip applying the changes in Ops Manager - defaults to `false`
* `PCF_USER` - ops manager username - defaults to `admin`
* `OPSMAN_CERT_NAME` - name of certificate in ops manager - defaults to `Certificate`
* `LE_SERVER` - Lets Encrypt server (optional) - will default to `https://acme-v02.api.letsencrypt.org/directory`
* `GCP_CREDENTIALS_FILE` - name of the file to store credentials, defaults to `/accounts.json`
* `GCP_CERT_NAME` - new name of certificate file in GCP - defaults to an auto generated UUID
* `GCP_DNS_WAIT` - how long to wait for DNS to propogate - defaults to 120 seconds
* `CERT_RENEW_BEFORE` - number of seconds the certificate may have left before renewing - defaults to 7 days (`604800`)

## Docker Hub
You can download it on Docker Hub too as `clijockey/le-pcf-on-gcp`

### Running the Docker Image
Assuming you have the above environment variables set:
```bash
docker run \
  -e GCP_CREDENTIALS="${GCP_CREDENTIALS}" \
  -e PCF_USER=${PCF_USER} \
  -e PCF_PASSWD=${PCF_PASSWD} \
  -e PCF_OPSMGR=${PCF_OPSMGR} \
  -e LE_EMAIL=${LE_EMAIL} \
  -e GCP_HTTPS_PROXY=${GCP_HTTPS_PROXY} \
  -e DOMAIN=${DOMAIN} \
  clijockey/le-pcf-on-gcp:latest
```


