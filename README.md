# 🛡️ CSPM Compliance MCP Server

> **A comprehensive Cloud Security Posture Management (CSPM) and Compliance Automation platform built using Model Context Protocol (MCP)**

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Python](https://img.shields.io/badge/python-3.9+-blue.svg)
![AWS](https://img.shields.io/badge/AWS-CSPM-orange.svg)
![HIPAA](https://img.shields.io/badge/HIPAA-Compliant-green.svg)
![SOC2](https://img.shields.io/badge/SOC2-Ready-green.svg)

## 🚀 Overview

This project provides an **end-to-end Cloud Security Posture Management (CSPM) solution** similar to tools like Scrut Automation and Prowler, but built using the **Model Context Protocol (MCP)** architecture. It automates compliance monitoring for frameworks like **HIPAA**, **SOC2**, **PCI-DSS**, and **NIST**, providing detailed control mapping, evidence collection, and automated remediation.

### ✨ Key Features

- 🔍 **Comprehensive CSPM Scanning** - Automated security posture assessment across AWS services
- 📋 **Multi-Framework Compliance** - Support for HIPAA, SOC2, PCI-DSS, NIST with detailed control mapping
- 🤖 **MCP Architecture** - Built using Model Context Protocol for seamless AI integration
- 📊 **Real-time Dashboard** - Interactive web interface for compliance monitoring
- ⚡ **Automated Remediation** - Smart remediation recommendations and auto-fix capabilities  
- 📈 **Risk Assessment** - Advanced risk scoring and prioritization
- 🔗 **Evidence Collection** - Comprehensive audit trail and evidence gathering
- 📝 **Detailed Reporting** - Executive summaries and technical compliance reports

## 🏗️ Architecture

```
┌─────────────────────┐    ┌─────────────────────┐    ┌─────────────────────┐
│   MCP Client        │◄──►│   MCP Server        │◄──►│   AWS Services      │
│   (Claude/AI)       │    │   (CSPM Engine)     │    │   (EC2, S3, IAM)    │
└─────────────────────┘    └─────────────────────┘    └─────────────────────┘
                                      │
                           ┌─────────────────────┐
                           │   Dashboard UI      │
                           │   (React/JS)        │
                           └─────────────────────┘
```

## 🛠️ Technology Stack

- **Backend**: Python 3.9+, MCP SDK, Boto3
- **Frontend**: HTML5, CSS3, JavaScript, Chart.js
- **Cloud**: AWS (EC2, S3, IAM, RDS, Security Hub, Config)
- **Compliance**: HIPAA, SOC2, PCI-DSS, NIST frameworks
- **Container**: Docker, Docker Compose

## 📦 Installation

### Prerequisites

- Python 3.9 or higher
- AWS Account with appropriate permissions
- AWS CLI configured
- Docker (optional, for containerized deployment)
- Git

### Quick Start

1. **Clone the Repository**
   ```bash
   git clone https://github.com/yourusername/cspm-compliance-mcp.git
   cd cspm-compliance-mcp
   ```

2. **Set Up Python Environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. **Configure AWS Credentials**
   ```bash
   aws configure
   # Enter your AWS Access Key ID, Secret Access Key, and default region
   ```

4. **Set Up Environment Variables**
   ```bash
   cp .env.example .env
   # Edit .env file with your configuration
   ```

5. **Initialize the Database and Configuration**
   ```bash
   python setup.py install
   python src/utils/setup_config.py
   ```

6. **Start the MCP Server**
   ```bash
   python src/main.py
   ```

7. **Launch the Dashboard**
   ```bash
   # In a new terminal
   cd dashboard
   python -m http.server 8080
   # Open http://localhost:8080 in your browser
   ```

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in the root directory:

```bash
# AWS Configuration
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_DEFAULT_REGION=us-east-1

# MCP Server Configuration
MCP_SERVER_PORT=3000
MCP_SERVER_HOST=localhost

# Compliance Configuration
HIPAA_ENABLED=true
SOC2_ENABLED=true
PCI_DSS_ENABLED=false
NIST_ENABLED=false

# Dashboard Configuration
DASHBOARD_PORT=8080
DASHBOARD_REFRESH_INTERVAL=300

# Logging
LOG_LEVEL=INFO
LOG_FILE=logs/cspm.log

# Auto-Remediation
AUTO_REMEDIATION_ENABLED=false
AUTO_REMEDIATION_DRY_RUN=true
```

### AWS Permissions

Your AWS user/role needs the following permissions:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:Describe*",
                "s3:Get*",
                "s3:List*",
                "iam:Get*",
                "iam:List*",
                "rds:Describe*",
                "config:Describe*",
                "config:Get*",
                "securityhub:Get*",
                "securityhub:List*",
                "securityhub:Describe*",
                "cloudtrail:Describe*",
                "cloudtrail:Get*",
                "kms:Describe*",
                "kms:Get*",
                "logs:Describe*"
            ],
            "Resource": "*"
        }
    ]
}
```

## 🎯 Usage

### 1. Running CSPM Scans

```python
# Using the MCP client
import asyncio
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_cspm_scan():
    server_params = StdioServerParameters(
        command="python", 
        args=["src/main.py"]
    )
    
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(read, write) as session:
            # Initialize the session
            await session.initialize()
            
            # Run security group scan
            result = await session.call_tool(
                "scan_security_groups",
                {
                    "region": "us-east-1",
                    "framework": "HIPAA"
                }
            )
            
            print(f"Scan Results: {result}")

# Run the scan
asyncio.run(run_csmp_scan())
```

### 2. Compliance Report Generation

```python
async def generate_hipaa_report():
    result = await session.call_tool(
        "generate_compliance_report",
        {
            "framework": "HIPAA",
            "scope": "account",
            "format": "JSON"
        }
    )
    return result
```

### 3. Auto-Remediation

```python
async def remediate_findings():
    result = await session.call_tool(
        "auto_remediate_findings",
        {
            "finding_ids": ["sg-unrestricted-001", "s3-encryption-002"],
            "dry_run": True,
            "approval_required": True
        }
    )
    return result
```

## 📊 Dashboard Features

The web dashboard provides:

- **Real-time Compliance Monitoring** - Live updates of compliance status
- **Framework-Specific Views** - HIPAA, SOC2, PCI-DSS specific dashboards
- **Interactive Filtering** - Filter by service, region, severity, status
- **Detailed Finding Analysis** - Drill-down into individual security findings
- **Remediation Workflow** - Track and manage remediation progress
- **Executive Reporting** - High-level summaries for stakeholders

## 🔧 Available MCP Tools

| Tool Name | Description | Framework Support |
|-----------|-------------|-------------------|
| `scan_security_groups` | Analyze security groups for misconfigurations | HIPAA, SOC2, NIST |
| `analyze_iam_policies` | Validate IAM policies for compliance | All frameworks |
| `check_s3_compliance` | S3 bucket security and encryption checks | HIPAA, SOC2, PCI-DSS |
| `monitor_rds_compliance` | RDS instance compliance monitoring | HIPAA, SOC2 |
| `generate_compliance_report` | Comprehensive compliance reporting | All frameworks |
| `check_hipaa_compliance` | HIPAA-specific compliance assessment | HIPAA |
| `audit_soc2_controls` | SOC2 Trust Services Criteria validation | SOC2 |
| `auto_remediate_findings` | Automated finding remediation | All frameworks |
| `create_remediation_plan` | Generate detailed remediation plans | All frameworks |
| `schedule_compliance_scan` | Schedule recurring compliance scans | All frameworks |

## 🧪 Testing

Run the test suite:

```bash
# Unit tests
pytest tests/unit/

# Integration tests
pytest tests/integration/

# End-to-end tests
pytest tests/e2e/

# Run all tests with coverage
pytest --cov=src tests/
```

## 🐳 Docker Deployment

### Using Docker Compose

```bash
# Build and start the application
docker-compose up -d

# View logs
docker-compose logs -f

# Stop the application
docker-compose down
```

### Individual Container

```bash
# Build the image
docker build -t cspm-compliance-mcp .

# Run the container
docker run -d \
  --name cspm-server \
  -p 3000:3000 \
  -p 8080:8080 \
  -e AWS_ACCESS_KEY_ID=your_key \
  -e AWS_SECRET_ACCESS_KEY=your_secret \
  cspm-compliance-mcp
```

## 📋 Compliance Framework Details

### HIPAA Support

- **164.308** - Administrative Safeguards
  - Assigned security responsibility
  - Workforce training and access management
  - Information systems activity review

- **164.310** - Physical Safeguards
  - Facility access controls
  - Workstation and device controls

- **164.312** - Technical Safeguards
  - Access control (unique user identification)
  - Audit controls and logging
  - Data integrity and authentication
  - Transmission security

- **164.314** - Organizational Requirements
  - Business associate contracts
  - Group health plan requirements

### SOC2 Support

- **CC1** - Control Environment
- **CC2** - Communication and Information
- **CC3** - Risk Assessment  
- **CC4** - Monitoring Activities
- **CC5** - Control Activities
- **CC6** - Logical and Physical Access Controls
- **CC7** - System Operations
- **CC8** - Change Management

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Related Projects

- [Prowler](https://github.com/prowler-cloud/prowler) - Open-source cloud security tool
- [Scrut Automation](https://scrut.io) - Commercial GRC platform
- [AWS Security Hub](https://aws.amazon.com/security-hub/) - AWS native security service
- [MCP](https://github.com/modelcontextprotocol/python-sdk) - Model Context Protocol

## 📞 Support

- **Documentation**: [docs/README.md](docs/README.md)
- **Issues**: [GitHub Issues](https://github.com/yourusername/cspm-compliance-mcp/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/cspm-compliance-mcp/discussions)

## 🏆 Acknowledgments

- Inspired by the excellent work of the [Prowler](https://github.com/prowler-cloud/prowler) project
- Built using the [Model Context Protocol](https://github.com/modelcontextprotocol) framework
- Compliance mappings based on official HIPAA, SOC2, and NIST guidelines

---

**⭐ Star this repository if you find it helpful!**
