# Aqua PDF Report Generator Action

## Usage

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

## Options

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
