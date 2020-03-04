# PKS Azure DNS Resolution

## What does this do?

This will ensure that PKS clusters on Azure have nodes that can be resolved by BOSH DNS.  This is necessary if you bring your own DNS server (not Azure DNS).

Requires Ops Manager 2.5.29+, 2.6.16+, 2.7.7+, 2.8.0+ (specifically it needs BOSH DNS 1.12+)


## How do I install it?

1. Open a shell prompt on a BOSH CLI with access to your PKS bosh director, such as Ops Manager.
2. Export your BOSH credentials to the enviornment.  These can be accessed via the Ops Manager GUI -> BOSH Director Tile -> Credentials Tab -> Bosh Commandline Credentials.

e.g.
```
export BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=fakesecret BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate  BOSH_ENVIRONMENT=10.0.0.10
```
3. Copy or clone this repository onto this BOSH CLI workstation and create+upload the BOSH release to the director

```
git clone https://github.com/svrc-pivotal/pks-azure-dns-nodes && cd pks-azure-dns-nodes
git submodule init ; git submodule update
cd os-conf-release
bosh create-release --force
bosh upload-release ./dev_releases/os-conf/os-conf-21.0.0+dev.1.yml

```
4. Configure the addon from this repo
```
cd ..
bosh -n update-config --name=pks-azure-dns --type=runtime ./addon.yml
```
5. Update your PKS clusters via the PKS CLI and/or Ops Manager "Apply Pending Changes" button with the PKS upgrade errand enabled.  This addon will automatically be installed on all worker nodes with the default manifest `pks-dpw-manifest.yml`

