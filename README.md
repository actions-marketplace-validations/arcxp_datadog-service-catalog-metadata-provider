# DataDog Service Catalog Metadata Provider

Welcome to the Datadog Service Catalog Metadata Provider!

## Supported Metadata Fields

Here's a simplified list of the metadata fields that are supported by the Datadog Service Catalog Metadata Provider:

| Field | Description | Required | Default |
| --- | --- | --- | --- |
| `datadog-key` | The Datadog API key to use for the integration. _Please_ use [Encrypted Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) to secure your secrets. | Yes | |
| `service-name` | The name of the service. This must be unique across all services. | Yes | |
| `team` | The team that owns the service. | Yes | |
| `email` | The email address of the team that owns the service. | Yes | |
| `slack-support-channel` | The Slack channel where folks can get support for the service. | Yes | |
| `repo` | The GitHub repository where the service is hosted. This is a convenience input for when you only have one repository for the service. You must either have this value, or you must have values specified in the `repos` object. | No | |
| `service-catalog-yaml-file` | If you'd prefer to use the native DataDog YAML file format, you can specify the path to the file here. If you supply this file, only this file will be used and all other values here are not. | No | |

Here's a larger set of metadata fields that are supported by the Datadog Service Catalog Metadata Provider:

| Field | Description | Required | Default |
| --- | --- | --- | --- |
| `contacts` | The list of contacts for the service. Each of these contacts is an object. Keep in mind that `email` and `slack-support-channel` are already included as contacts. This list should be in addition to that. These values are supplied as objects, but due to the limitations of GitHub Actions, please supply these object properties as a multi-line string. | No | |
| `contacts[].name` | The name of the contact. | Yes | |
| `contacts[].type` | The type of the contact. Acceptable values are: `email`, `slack`, and `microsoft-teams` | Yes | |
| `contacts[].value` | The value of the contact. For example, if the type is `email`, this would be the email address. | Yes | |
| `repos` | The list of GitHub repositories that are part of the service. You must supply at least one repository. The repositories are supplied as objects, but due to the limitations of GitHub Actions, please supply these object properties as a multi-line string. | Yes | |
| `repos[].name` | The name of the repository. | Yes | |
| `repos[].url` | The URL of the repository. | Yes | |
| `repos[].provider` | The provider of the repository. Acceptable values are: `Github`. | No | `Github` |
| `tags` | The list of tags that are associated with the service. This should be a list of key-value pairs separated by colons. | No | |
| `links` | A list of links associated with the service. These links are objects with a variety of properties, but due to the limitations of GitHub Actions, please supply these object properties as a multi-line string. | No | |
| `links[].name` | The name of the link. | Yes | |
| `links[].url` | The URL of the link. | Yes | |
| `links[].type` | The type for the link. Acceptable values are: `doc`, `wiki`, `runbook`, `url`, `repo`, `dashboard`, `oncall`, `code`, and `link` | Yes | |
| `docs` | A list of documentation links associated with the service. These links are objects with a variety of properties, but due to the limitations of GitHub Actions, please supply these object properties as a multi-line string. | No | |
| `docs[].name` | The name of the document. | Yes | |
| `docs[].url` | The URL of the document. | Yes | |
| `docs[].provider` | The provider for where the documentation lives. Acceptable values are: `Confluence`, `GoogleDocs`, `Github`, `Jira`, `OneNote`, `SharePoint`, and `Dropbox` | No | |

## Example

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: arcxp/datadog-service-catalog-metadata-provider@v1
        with:
          datadog-key: ${{ secrets.DATADOG_API_KEY }}
          service-name: hello-world
          team: Global Greetings
          email: global.greetings@fake-email-host.com
          slack-support-channel: '#global-greetings-support'
          repos:
            - name: hello-world
              url: https://github.com/fake-org/hello-world
              provider: github
          docs:
            - name: outage-runbook
              url: https://fake-org.github.io/hello-world-outage-runbook
              provider: github
```

## References

- [DataDog Service Definition API](https://docs.datadoghq.com/tracing/service_catalog/service_definition_api/)
- [JSON Schema for the DataDog Service Definition](https://github.com/DataDog/schema/blob/main/service-catalog/v2/schema.json)
