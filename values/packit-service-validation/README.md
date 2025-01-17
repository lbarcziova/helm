## Install

Login to [PSI](https://ocp4.psi.redhat.com) and switch to `cyborg` project.

    oc login --token=sha256~....  --server= ....
    oc project cyborg

Get secrets from Bitwarden.
Sentry from `extra-vars.yml` in `secrets-packit-[prod|stg]` item and
GitHub token from `Release/usercont bot` item.

    export SENTRY=$( echo -n 'token from bitwarden' | base64 )
    export GITHUB=$( echo -n 'token from bitwarden' | base64 )

### Install from this repo

    make install DEPLOYMENT=[production|staging]

### Install from chart repository

If you're going to use the chart from outside (without having this repo cloned),
you can install the chart from our chart repository. You just need to have a file
with keys overriding those defined in `values.yaml` with `~` value.

    helm repo add packit https://helm.packit.dev
    helm repo update
    helm upgrade --install --cleanup-on-fail packit-service-validation packit/packit-service-validation --set secrets.sentry=${SENTRY} --set secrets.github=${GITHUB} --values your-values-file.yaml

### Render templates

If you just want to see how the rendered templates would look like:

    make dryrun DEPLOYMENT=[production|staging]
