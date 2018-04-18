# Montagu Kodiak
Thin wrapper around [Kodiak repo](https://github.com/vimc/kodiak) to add Montagu
specific bits.

## Disaster recovery
A Montagu specific script: `restore-vault`. This is run on the support machine
in a complete disaster recovery scenario.

This script gets the data from S3 into the correctly named volume for Vault to
be run as normal.
```
#!/usr/bin/env bash
# This config file only has the vault target, and has a temporary path for the
# starport.
kodiak setup ../config/vault-disaster-recovery.json

# Get the data from S3
kodiak restore vault

# Move it into the right volume
docker volume create montagu_vault_data
docker run bash \
    -v montagu_vault_data:/volume \
    -v $TEMP_STARPORT/plain/vault:/starport \
    cp /starport/* /volume

echo "Now run the vault as normal following instructions at"
echo "https://github.com/vimc/montagu-vault"
```
