
# Janus Cluster using Ansible

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
