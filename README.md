# Vault pki roles, policies and kubernetes auth role:


```
/ # vault policy read pki
path "pki*"                        { capabilities = ["read", "list", "update", "create"] }

Key                                   Value
---                                   -----
allow_any_name                        false
allow_bare_domains                    false
allow_glob_domains                    false
allow_ip_sans                         true
allow_localhost                       true
allow_subdomains                      true
allow_token_displayname               false
allowed_domains                       [my-website.com]
allowed_domains_template              false
allowed_other_sans                    []
allowed_serial_numbers                []
allowed_uri_sans                      []
basic_constraints_valid_for_non_ca    false
client_flag                           true
code_signing_flag                     false
country                               []
email_protection_flag                 false
enforce_hostnames                     true
ext_key_usage                         []
ext_key_usage_oids                    []
generate_lease                        false
key_bits                              2048
key_type                              rsa
key_usage                             [DigitalSignature KeyAgreement KeyEncipherment]
locality                              []
max_ttl                               72h
no_store                              false
not_before_duration                   30s
organization                          []
ou                                    []
policy_identifiers                    []
postal_code                           []
province                              []
require_cn                            false
server_flag                           true
street_address                        []
ttl                                   0s
use_csr_common_name                   true
use_csr_sans                          true
/ # 
/ # vault read auth/kubernetes/role/issuer
Key                                 Value
---                                 -----
bound_service_account_names         [internal-app]
bound_service_account_namespaces    [default]
policies                            [pki]
token_bound_cidrs                   []
token_explicit_max_ttl              0s
token_max_ttl                       0s
token_no_default_policy             false
token_num_uses                      0
token_period                        0s
token_policies                      [pki]
token_ttl                           20m
token_type                          default
ttl                                 20m

/ # vault read auth/kubernetes/role/issuer-1
Key                                 Value
---                                 -----
bound_service_account_names         [internal-app]
bound_service_account_namespaces    [openstack]
policies                            [pki]
token_bound_cidrs                   []
token_explicit_max_ttl              0s
token_max_ttl                       0s
token_no_default_policy             false
token_num_uses                      0
token_period                        0s
token_policies                      [pki]
token_ttl                           20m
token_type                          default
ttl                                 20m
```
===============
**Note:** To get the traefik part working, make sure to change the targetPort for the mgmt service to https and restart pod to reflect the changes otherwise you will get this error:
```
(pyenv-ansible-2.6) [dev] root@foreman:oiaas-deployment-ansible# curl https://barbican.oiaas.com
curl: (35) SSL received a record that exceeded the maximum permissible length.
```
 and to delete traefik pods again otherwise you will get this error:
 ```
* About to connect() to barbican.oiaas.com port 443 (#0)
*   Trying 192.168.1.184...
* Connection refused
* Failed connect to barbican.oiaas.com:443; Connection refused
* Closing connection 0
curl: (7) Failed connect to barbican.oiaas.com:443; Connection refused
```


