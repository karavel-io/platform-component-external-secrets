# Configuration

```hcl
component "external-secrets" {
  namespace = "external-secrets" # optional

  # Params default values

  defaultProvider = "" # required, what secret store to use, available: "aws", "vault"

  # required when using AWS Secret Manager
  aws = {
    enable = false       # required, set to true if using AWS
    region = "eu-west-1" # required, AWS region, defaults to "eu-west-1"
    eksRole = ""         # optional, EKS role ARN
    iamRole = ""         # optional, IAM role ARN
  }

  # required when using Hashicorp Vault
  vault = {
    enable = false                                         # required, set to true when using Vault
    address = "https://vault.vault.svc.cluster.local:8200" # required, URL to Vault instance
    namespace = ""                                         # required, namespace of Vault instance

    # optional, override authentication method (only Kubernetes SATs are supported)
    auth = {
      kubernetes = {
        mountPath = "kubernetes"
        role = "karavel"
      }
    }

    # optional, override secret engine and path
    engine = {
      version = "v2"
      path = "secret"
    }
  }
}
```

## AWS Secret Manager

To use AWS Secret Manager, you will need to create a valid IAM role with the following read-only policy:

- `secretsmanager:GetResourcePolicy`
- `secretsmanager:GetSecretValue`
- `secretsmanager:DescribeSecret`
- `secretsmanager:ListSecretVersionIds`
