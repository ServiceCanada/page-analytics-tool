# Page Analytics Tool (PAT)

# Table of Contents #
  * [Setup](#setup)
  * [What is needed](#what-is-needed)

## Setup

* Update */php/config.php* with the values of your API
* Include a */keys/* folder with a *secret.pem*.

## What is needed

* PHP 7.2 and above
  * If that cannot be provided, above PHP 5.5.
* PHP with the extensions enabled:
  * curl;
  * openssl; and
  * mongodb
* OpenSSL above 1.0.1 (or latest)
* MongoDB

## Kubernetes deployment

The production deployments require the `tbsacr-pull` Docker registry secret in
the `performance` namespace. The secret must authenticate to
`tbsacr.azurecr.io` with the least-privileged `AcrPull` role.

The secret is provisioned outside this repository; never commit its credential.
Rotate the backing credential before it expires and update the Kubernetes secret
without changing its name. Both deployment manifests reference `tbsacr-pull`, so
future GitHub Actions deployments retain explicit registry authentication.
