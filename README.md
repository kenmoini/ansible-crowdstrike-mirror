# ansible-crowdstrike-mirror

[![Ansible Lint](https://github.com/kenmoini/ansible-crowdstrike-mirror/actions/workflows/ansible-lint.yml/badge.svg)]((https://github.com/kenmoini/ansible-crowdstrike-mirror/actions/workflows/ansible-lint.yml))
[![Execution Environment](https://github.com/kenmoini/ansible-crowdstrike-mirror/actions/workflows/build-deploy-ee.yml/badge.svg)](https://github.com/kenmoini/ansible-crowdstrike-mirror/actions/workflows/build-deploy-ee.yml)
[![Get Execution Environment on Quay](https://img.shields.io/badge/Quay.io-Get%20EE%20Image-blue)](https://quay.io/kenmoini/ansible-crowdstrike-mirror-ee)

This set of Ansible Content will mirror the CrowdStrike Falcon Sensor container images from their registry into your own private registry.

It does this by:

1. Authenticating to the CrowdStrike API with an OAuth2 Client ID & Client Secret and requesting an active Bearer Token
2. Authenticating to the CrowdStrike API with the active Bearer Token to get a Image Registry Token
3. Logging into the CrowdStrike Image Registry
4. Logging into your Private Image Registry
5. Retrieving all of the active falcon-sensor image tags
6. Looping through those tags and pulling, re-tagging, and pushing them to the Private Image Registry
7. ???????
8. PROFIT!!!!!1

## Prerequisites

- Ansible Automation - `python3 -m pip install ansible`
- Skopeo - `dnf install skopeo -y`
- A Private Image Registry with permissions to push images

## Getting Started

1. Install the needed pip modules: `python3 -m pip install -r requirements.txt`
2. Install the needed Ansible Collections: `ansible-galaxy collections install -r collections/requirements.yml`
3. Run the Playbook: 

```bash
ansible-playbook -i inventory \
 -e crowdstrike_client_id="$(cat /root/.cs-client-id)" \
 -e crowdstrike_client_secret="$(cat /root/.cs-client-secret)" \
 -e crowdstrike_customer_id="$(cat /root/.cs-cid)" \
 -e remote_registry_endpoint="jfrog.kemo.labs" \
 -e remote_registry_image_path="cs-mirror/falcon-sensor" \
 -e remote_registry_username="robot-mirror" \
 -e remote_registry_password="Passw0rd123!" \
 mirror.yml
```

## Using in Ansible Controller/Tower

- Create a new Custom Credential for the CrowdStrike OAuth 2 Client ID/Secret - see in [./docs/ansible-custom-credentials.md](./docs/ansible-custom-credentials.md)
- Create said Credential of Custom Credential Type
- Create an Execution Environment with the upstream EE: `quay.io/kenmoini/ansible-crowdstrike-mirror-ee:latest`
- Create a Project with this repo, set it to use the EE
- Create a Job Template, associate a localhost Inventory, the Credential, set it to use the EE, enable Privilege Escalation, and select the `mirror.yml` Playbook.
- Provide it with the additional variables to push to your private image registry
- Set it to run on a Schedule
