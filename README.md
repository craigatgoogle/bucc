# BUCC ([BOSH](http://bosh.io/), [UAA](https://github.com/cloudfoundry/uaa), [Credhub](https://github.com/cloudfoundry-incubator/credhub) and [Concourse](http://concourse.ci/)) [![BUCC CI](https://ci.starkandwayne.com/api/v1/pipelines/bucc/jobs/test/badge)](https://ci.starkandwayne.com/teams/main/pipelines/bucc)

The bucc command line utility allows for easy bootstrapping of the BUCC stack (Bosh Uaa Credhub and Concourse). Which is the starting point for many deployments.

## Install the bucc-cli

### Prepare the Environment

1. Install [BOSH CLI v2.0.1+](https://bosh.io/docs/cli-v2.html)

2. Optionall install [`direnv`](https://direnv.net/)

3. Clone this repository

```
git clone https://github.com/starkandwayne/bucc.git
cd bucc
```

### Boot your BUCC VM

Choose your cpi:
```
bucc up --help
  --cpi    Cloud provider: [aws, azure, docker, gcp, openstack, virtualbox, vsphere]
  --lite   Created bosh will use the warden cpi with garden runc

  Optional cpi specific flags:
    gcp: --service-account
    vsphere: --dns --resource-pool
```

From the repo root run:
```
$ bucc up --lite
```

To delete your VM run:
```
$ bucc down
```

## Using BUCC

### Using BOSH

```
$ source <(bucc env) # should not be necessary when using direnv

$ bosh alias-env bucc
  Using environment '192.168.50.6' as client 'admin'

  Name      Bosh
  Director
  UUID      3e107016-3fc2-40af-8ac5-8e53025d53f3
  Version   260.5.0 (00000000)
  CPI       virtualbox_cpi
  Features  compiled_package_cache: disabled
            dns: disabled
            snapshots: disabled
  User      admin

  Succeeded

$ bosh vms
  Using environment '192.168.50.6' as client 'admin'

  Succeeded
```

### Using UAA

1. Install the cli

```
gem install cf-uaac
```

2. Use UAA

```
$ bucc uaac

  Target: https://192.168.50.6:8443
  Context: uaa_admin, from client uaa_admin


  Successfully fetched token via client credentials grant.
  Target: https://192.168.50.6:8443
  Context: uaa_admin, from client uaa_admin

$ uaac client get admin
  scope: uaa.none
  client_id: admin
  resource_ids: none
  authorized_grant_types: client_credentials
  autoapprove:
  authorities: bosh.admin
  lastmodified: 1490280436993
```

### Using Credhub

```
$ bucc credhub
  Setting the target url: https://192.168.50.6:8844
  Login Successful

$ credhub generate -t password --name test
  Type:          password
  Name:          /test
  Value:         Nfjbu0HKKI9eHmbGY6hNLjssDphpdO
  Updated:       2017-03-23T14:49:03Z
```

### Using Concourse

#### Via the GUI

To get the login details for your concourse GUI run:

```
bucc info
```


#### With Fly

```
$ bucc fly

  target saved

$ fly -t bucc pipelines
  name  paused  public
```
