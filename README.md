# PDQ Connect

`PDQ Connect` is a Windows Service API that provides a REST interface to interact with [PDQ Deploy](https://www.pdq.com/pdq-deploy-and-inventory/). It allows remote triggering of deployments, monitoring, and status retrieval for software installations across a network.

## 🔧 Features

- Trigger PDQ Deploy packages via REST
- List available packages
- Monitor deployment status
- Designed to run as a Windows Service

## 📦 Technologies

- PowerShell / Python / .NET (dependendo do que usou)
- PDQ Deploy CLI
- NSSM (for service setup)

## 🚀 Getting Started

1. Install PDQ Deploy on your machine
2. Clone this repository
3. Setup the service using `nssm` or another method
4. Configure credentials and endpoints
5. Start interacting via HTTP calls

## 📄 License

[MIT](LICENSE)
