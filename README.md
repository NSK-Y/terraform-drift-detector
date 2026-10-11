# Terraform Drift Detection Platform

A Go-based infrastructure tool for identifying configuration drift between Terraform state and live cloud resources.

This repository is a personal learning and development project based on the open-source [terraform-drift-detector](https://github.com/iam-veeramalla/terraform-drift-detector) project. The goal is to extend the existing implementation with additional testing, automation, security checks, and operational capabilities.

## Overview

Infrastructure drift occurs when deployed cloud resources differ from the configuration tracked by Terraform. This project compares expected resource state with information retrieved from cloud provider APIs and generates reports highlighting detected differences.

## Existing Features

The inherited implementation includes:

- Terraform state reading from local files or S3.
- AWS resource inspection for EC2, VPC, subnets, security groups, and S3.
- Drift detection for missing resources, unexpected resources, attribute changes, and tag changes.
- Command-line interface for running scans.
- REST API for triggering scans and retrieving reports.
- Scheduled scans using cron expressions.
- Web dashboard and SQLite persistence.
- JSON and table-based reporting.

## Architecture

```text
Terraform State ──> State Reader ──> Expected Resources ──┐
                                                         ├──> Drift Engine
AWS Cloud APIs ───> Cloud Fetcher ──> Actual Resources ──┘
                                                               |
                                                               v
                                                        Drift Report
```

## Technology Stack

- **Language:** Go
- **Infrastructure as Code:** Terraform
- **Cloud Provider:** AWS
- **API:** REST
- **Database:** SQLite
- **Frontend:** Existing web dashboard
- **Automation:** Makefile and command-line tooling

## Getting Started

### Prerequisites

- Go compatible with the version specified in `go.mod`
- Git
- AWS credentials and appropriate IAM permissions for live AWS scans
- Terraform state file for the infrastructure being inspected

### Clone this repository

```bash
git clone https://github.com/NSK-Y/terraform-drift-detector.git
cd terraform-drift-detector
```

### Build

```bash
go build -o bin/driftctl ./cmd/driftctl
```

### Run a state-only scan

```bash
./bin/driftctl scan --state testdata/state/sample.tfstate --provider aws --skip-cloud
```

### Run the API server

Review the configuration and available Makefile targets before starting the server:

```bash
make run-server
```

If the server starts successfully, open `http://localhost:8080` in your browser.

### AWS configuration

Before performing live scans, configure AWS credentials securely and verify that the IAM identity has the minimum permissions required by the resource types being inspected.

Never commit access keys, secret keys, Terraform state containing sensitive data, or other credentials to Git.

## Project Structure

```text
.
├── cmd/          # Application entry points
├── configs/      # Application configuration
├── internal/     # Core application implementation
├── testdata/     # Test fixtures
├── web/          # Web dashboard assets
├── go.mod        # Go dependencies and module definition
├── Makefile      # Build and run targets
├── README.md     # Project documentation
└── LICENSE       # Apache 2.0 license
```

## Development Roadmap

Planned improvements for this portfolio project:

- Automated unit and integration tests.
- GitHub Actions CI pipeline for builds and tests.
- Static analysis and dependency vulnerability scanning.
- Improved drift report filtering and severity classification.
- Prometheus metrics and operational monitoring.
- Containerized deployment and deployment documentation.

These are planned improvements, not claims that the features are already implemented. Completed changes will be documented as they are built and tested.

## Upstream Attribution

This project is based on the open-source work in [iam-veeramalla/terraform-drift-detector](https://github.com/iam-veeramalla/terraform-drift-detector).

The original project's applicable copyright notices and license terms are retained. New contributions and modifications will be documented as development progresses.

## License

This project is distributed under the Apache License 2.0, subject to the terms of the included [`LICENSE`](LICENSE) file and applicable upstream notices.

See the original project for upstream history and contributions:
https://github.com/iam-veeramalla/terraform-drift-detector
