# Aqua PDF Report Generator Action
This action generates PDF reports for Aqua CSP image and host vulnerabilities.

# Usage

```yaml
name: Aqua Report Generator
on: [push]

jobs:
  pdf-report-generator:
    runs-on: ubuntu-latest
    steps:
    - name: Generate PDF report
      uses: rcarpio-hbo/aqua-reportgen-action@v0.1.0
      with:
        server: "https://cloud.aquasec.com/"
        user: ${{ secrets.aqua_username }}
        password: ${{ secrets.aqua_password }}
        registry: "Docker%20Hub"
        image: "node:12.22.10"
```

# Options

| Option            | Required      |   Default  | Description                                                                 |
|:-----------------:|:-------------:|:----------:|---------------------------------------------------------------------------- |
|  server           |      true     |     -      | Aqua CSP server URL                                                         |
|  user             |      true     |     -      | Aqua CSP Username                                                           |
|  password         |      true     |     -      | Aqua CSP Password                                                           |
|  report_filename  |      true     | report.pdf | Output filename to save PDF                                                 |
|  registry         |      false    |     -      | Image Registry                                                              |
|  image            |      false    |     -      | Image name to generate report for (e.g. mongo:latest)                       |
|  host             |      false    |     -      | Host name to generate report for                                            |
|  severity         |      false    |     -      | Comma separated list of severities to export (critical, high, medium, low)" |

# Scenarios
* [Upload PDF report as build artifact](#upload-pdf-report-as-build-artifact)
* [Send PDF to Slack](#send-pdf-to-slack)

## Upload PDF report as build artifact
Upload the PDF report as Github artifact to be share between jobs and store data once a workflow is complete.

```
steps:
- name: Generate PDF report
  uses: rcarpio-hbo/aqua-reportgen-action@v0.1.0
  with:
    server: "https://cloud.aquasec.com/"
    user: ${{ secrets.aqua_username }}
    password: ${{ secrets.aqua_password }}
    registry: "Docker%20Hub"
    image: "node:12.22.10"
- name: Archive report
  uses: actions/upload-artifact@v3
  with:
    # Artifact name
    name: aqua-report
    # A file path that describes what to upload
    path: "/tmp/report.pdf"
    # Duration after which artifact will expire in days
    retention-days: 1
    # The desired behavior if no files are found using the provided path
    if-no-files-found: error
```

## Send PDF to Slack
Upload the PDF report as Github artifact to Slack.

```
steps:
- name: Generate PDF report
  uses: rcarpio-hbo/aqua-reportgen-action@v0.1.0
  with:
    server: "https://cloud.aquasec.com/"
    user: ${{ secrets.aqua_username }}
    password: ${{ secrets.aqua_password }}
    registry: "Docker%20Hub"
    image: "node:12.22.10"
- name: Upload to slack
  uses: adrey/slack-file-upload-action@master
  with:
    # The message text introducing the file in specified channels.
    initial_comment: "Aqua Report ${{ matrix.subservice.fullTag }}. Critical and high vulnerabilities must be fixed."
    # Slack app token https://github.com/marketplace/actions/slack-file-upload#token
    token: "${{ secrets.slack_token}}"
    # Path to file
    path: "/tmp/report.pdf"
    # Slack channel for upload
    channel: "${{ inputs.slack_channel }}"
```

# License
The scripts and documentation in this project are released under the [MIT License](LICENSE)