
# Janus Cluster using Ansible

The primary aim of this is to allow a fully automated fresh creation and setup of a Janus cluster in EC2.  Everyone has different needs - this is just to get your started.

## Parameters

Ensure the following variables are passed in as extra parameters:
- `aws_region` (e.g. ap-southeast-2)
- `env_name` (e.g. test, production, etc)

`ansible-playbook -i inventory deploy-janus.yml -e aws_region=ap-southeast-2 -e env_name=test`

## Requirements

Based on the above parameters, you need at least the region config file, e.g: `region/janus-ap-southeast-2-test.yml`

See `region/janus-region-example.yml` for more information on the config.

## Keys

By default it will create a private/public key pair for you in the `region/` directory based on the above parameters, e.g:
- `janus-ap-southeast-2-test.key`
- `janus-ap-southeast-2-test.key.pub`

If you don't want it to create a new key simply link or copy existing files into the above location.

## SSH Access

Ansible will use the location of the keys above to SSH onto the new servers.

## Notes

- Most of this is purely intended for a quick spin up of a cluster (single or multi AZ), it doesn't try to accomodate every possible combination of use.
- It modifies a few startup scripts because the defaults don't factor in clusters (e.g. Gremlin needs QUORUM but doesn't check it has it before starting)
- We store data on a separate disk to the commit log for performance

## Config

See `roles/janus-install/defaults/main.yml` for some other default cluster settings (these can be overriden on command line if you'd like):

- `cassandra_replication: 3` (3 is a useful number, but depends on your cluster size)
- `elasticsearch_replicas:` (defaults to 1/4 of your cluster size)

`roles/janus-install/templates/conf/gremlin-server/janusgraph-cassandra-es-server.properties.j2` also contains some default graph DB settings which can be tweaked:

- `storage.cassandra.read-consistency-level=` (set to `LOCAL_ONE` for multi-az deployments, or `ONE` for single cluster setups)
- `storage.cassandra.write-consistency-level=` (as above)

