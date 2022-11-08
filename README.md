# vault-init

The `vault-init` service automates the process of [initializing](https://www.vaultproject.io/docs/commands/operator/init.html) and [unsealing](https://www.vaultproject.io/docs/concepts/seal.html#unsealing) HashiCorp Vault instances running in development environments where you can't use dev mode as you want your secrets to survive a restart of the vault container.

After `vault-init` initializes a Vault server it stores master keys and root tokens, in plaintext in a file, that could be located
in e.g. a persistent volume of some sort in a containerized dev environment. 

## Usage

The `vault-init` service is designed to be run alongside a Vault server and
communicate over local host.

You can download the code and compile the binary with Go.

## Configuration

The `vault-init` service supports the following environment variables for configuration:

- `CHECK_INTERVAL` ("10s") - The time duration between Vault health checks. Set
  this to a negative number to unseal once and exit.

- `VAULT_SECRETS_PLAINTEXT_PATH` - the path where to write the root token and unseal keys.

- `VAULT_SECRET_SHARES` (5) - The number of human shares to create.

- `VAULT_SECRET_THRESHOLD` (3) - The number of human shares required to unseal.

- `VAULT_AUTO_UNSEAL` (true) - Use Vault 1.0 native auto-unsealing directly. You must
  set the seal configuration in Vault's configuration.

- `VAULT_STORED_SHARES` (1) - Number of shares to store on KMS. Only applies to
  Vault 1.0 native auto-unseal.

- `VAULT_RECOVERY_SHARES` (1) - Number of recovery shares to generate. Only
  applies to Vault 1.0 native auto-unseal.

- `VAULT_RECOVERY_THRESHOLD` (1) - Number of recovery shares needed to trigger an auto-unseal.
  Only applies to Vault 1.0 native auto-unseal.

- `VAULT_SKIP_VERIFY` (false) - Disable TLS validation when connecting. Setting
  to true is highly discouraged.

- `VAULT_CACERT` ("") - Path on disk to the CA _file_ to use for verifying TLS
  connections to Vault.

- `VAULT_CAPATH` ("") - Path on disk to a directory containing the CAs to use
  for verifying TLS connections to Vault. `VAULT_CACERT` takes precedence.

- `VAULT_TLS_SERVER_NAME` ("") - Custom SNI hostname to use when validating TLS
  connections to Vault.
