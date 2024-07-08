# Open Component Model (OCM) Hello World

## Create component version in local CTS (common-transport-archive)

`ocm add componentversions` directly creates or extends a common transport archive without the need for creating dedicated component archives

```sh
export COMPONENT="github.com/breuerjo/ocm-hello-world"
export VERSION="1.0.0"
export CTF_ARCHIVE="ocm-archive.ctf"
export OCM_REPO="ghcr.io/breuerjo/ocm-hello-world"
export REMOTE_CV_ARCHIVE_NAME="remote-cv-archive"

# create component version from local component constructor
ocm add componentversions --create --file $CTF_ARCHIVE component-constructor.yaml

# get components versions from CTS folder
ocm get componentversion $CTF_ARCHIVE

# get recursive information of CTS archive
ocm get resources $CTF_ARCHIVE --recursive -o tree

# transport component version to remote OCI registry
ocm transfer ctf -f $CTF_ARCHIVE --copy-resources $OCM_REPO # --copy-resources flag configures that also the external images are fetched and included in the ocm artefact
```

## Sign CTS archive (requires local RSA key pair)

```sh
ocm create rsakeypair keys/acme.priv
```

```sh
# sign CTS archive before transport
ocm sign componentversion --signature acme-sig --private-key=keys/acme.priv $CTF_ARCHIVE
```

## Download cts archive from remote OCI-registry

```sh
ocm download componentversions ${OCM_REPO}//${COMPONENT}:${VERSION} -O $REMOTE_CV_ARCHIVE_NAME
```

```sh
# get component version from downloaded component version
ocm get componentversion $REMOTE_CV_ARCHIVE_NAME
```

## Verify signature of downloaded cts archive (requires local RSA key pair)

```sh
ocm verify componentversions --signature acme-sig --public-key=keys/acme.pub $REMOTE_CV_ARCHIVE_NAME
```
