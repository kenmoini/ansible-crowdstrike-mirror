# Ansible Controller - Custom Credentials

To support this Ansible Content, it's best to create a Custom Credential Type in Ansible Controller/Tower.

- Credential Name: `crowdstrike_oauth2`
- Credential Description: `CrowdStrike OAuth 2 Client ID & Secret`
- Input Configuration:

```yaml
fields:
  - id: cs_client_id
    type: string
    label: Crowdstrike Client ID
  - id: cs_client_secret
    type: string
    label: Crowdstrike Client Secret
    secret: true
required:
  - cs_client_id
  - cs_client_secret
```

- Injector Configuration:

```yaml
extra_vars:
  crowdstrike_client_id: '{{ cs_client_id }}'
  crowdstrike_client_secret: '{{ cs_client_secret }}'
```