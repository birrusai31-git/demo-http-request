# CI/CD Pipeline Setup for MuleSoft Demo HTTP Request

This document provides a comprehensive guide for the CI/CD pipeline setup for the MuleSoft demo-http-request project.

## 🏗️ Pipeline Architecture

### Workflows Overview

1. **CI Workflow** (`.github/workflows/ci.yml`)
   - Triggers: Push to `main` or `develop`, PRs to `main`
   - Actions: Build, test, package, upload artifacts

2. **CD Workflow** (`.github/workflows/cd.yml`)
   - Triggers: Push to `main`, manual dispatch
   - Actions: Deploy to CloudHub 2.0 environments

3. **PR Validation** (`.github/workflows/pr-validation.yml`)
   - Triggers: Pull requests to `main` or `develop`
   - Actions: Code quality checks, build validation

## 🔧 Required GitHub Secrets

Navigate to **Settings > Secrets and variables > Actions** in your repository and add:

### Required Secrets

| Secret Name | Description | Example |
|-------------|-------------|----------|
| `ANYPOINT_USERNAME` | Anypoint Platform username | `your.email@company.com` |
| `ANYPOINT_PASSWORD` | Anypoint Platform password | `your-secure-password` |
| `ANYPOINT_BUSINESS_GROUP` | Business Group ID (optional) | `your-business-group` |

### Optional Secrets (for enhanced features)

| Secret Name | Description | Purpose |
|-------------|-------------|----------|
| `SONAR_TOKEN` | SonarQube token | Code quality analysis |
| `SONAR_HOST_URL` | SonarQube server URL | Code quality analysis |
| `SLACK_WEBHOOK` | Slack webhook URL | Deployment notifications |

## 🚀 Deployment Environments

The CD pipeline supports three environments:

### Development Environment
- **Trigger**: Automatic on push to `main`
- **Resources**: 1 MICRO worker
- **Region**: us-east-1
- **App Name**: `demo-http-request-dev`

### Test Environment
- **Trigger**: Manual dispatch
- **Resources**: 1 SMALL worker
- **Region**: us-east-1
- **App Name**: `demo-http-request-test`

### Production Environment
- **Trigger**: Manual dispatch
- **Resources**: 2 MEDIUM workers
- **Region**: us-east-1
- **App Name**: `demo-http-request-prod`

## 📋 Workflow Details

### CI Workflow Features

✅ **Build Validation**
- Compile MuleSoft application
- Validate XML configurations
- Check dependencies

✅ **Testing**
- Run MUnit tests
- Generate test reports
- Upload test artifacts

✅ **Quality Checks**
- SonarQube analysis (optional)
- Security scanning
- Code coverage reports

✅ **Artifact Management**
- Package application JAR
- Upload build artifacts
- 30-day retention policy

### CD Workflow Features

🚀 **Multi-Environment Support**
- Environment-specific configurations
- Resource allocation per environment
- Environment protection rules

🔧 **CloudHub 2.0 Integration**
- Automatic deployment
- Configuration management
- Deployment verification

📊 **Deployment Reporting**
- Deployment summary
- Environment details
- Success/failure notifications

## 🛠️ Setup Instructions

### Step 1: Configure Repository Secrets

```bash
# Navigate to your repository
# Go to Settings > Secrets and variables > Actions
# Add the required secrets listed above
```

### Step 2: Configure Anypoint Platform

1. Ensure you have access to Anypoint Platform
2. Create or identify target environments (Development, Test, Production)
3. Note your Business Group ID if using organizational setup

### Step 3: Update POM Configuration (if needed)

Ensure your `pom.xml` includes the CloudHub 2.0 plugin:

```xml
<plugin>
    <groupId>org.mule.tools.maven</groupId>
    <artifactId>mule-maven-plugin</artifactId>
    <version>${mule.maven.plugin.version}</version>
    <extensions>true</extensions>
    <configuration>
        <cloudhub2Deployment>
            <uri>https://anypoint.mulesoft.com</uri>
            <provider>MC</provider>
            <environment>${cloudhub2.environment}</environment>
            <target>${cloudhub2.target}</target>
            <muleVersion>${app.runtime}</muleVersion>
            <applicationName>${cloudhub2.applicationName}</applicationName>
            <replicas>${cloudhub2.replicas}</replicas>
            <vCores>${cloudhub2.vCores}</vCores>
        </cloudhub2Deployment>
    </configuration>
</plugin>
```

## 🔄 Workflow Usage

### Automatic CI

1. **Push to develop**: Triggers CI workflow
2. **Create PR to main**: Triggers PR validation
3. **Push to main**: Triggers CI + automatic deployment to dev

### Manual Deployment

1. Go to **Actions** tab in GitHub
2. Select **CD - Deploy to CloudHub**
3. Click **Run workflow**
4. Choose:
   - Environment (dev/test/prod)
   - Application name (optional)
5. Click **Run workflow**

### Branch Protection (Recommended)

Set up branch protection for `main`:

1. Go to **Settings > Branches**
2. Add rule for `main` branch
3. Enable:
   - Require pull request reviews
   - Require status checks to pass
   - Require up-to-date branches
   - Include administrators

## 📊 Monitoring and Troubleshooting

### Workflow Monitoring

- **Actions Tab**: View all workflow runs
- **Deployment Summary**: Check deployment details
- **Artifacts**: Download build artifacts
- **Logs**: Review detailed execution logs

### Common Issues

| Issue | Solution |
|-------|----------|
| Authentication failed | Check ANYPOINT_USERNAME and ANYPOINT_PASSWORD secrets |
| Deployment timeout | Increase timeout in workflow or check CloudHub status |
| Build failed | Check Maven dependencies and Mule configuration |
| Tests failed | Review MUnit test logs and fix failing tests |

### Debugging Steps

1. **Check Secrets**: Ensure all required secrets are configured
2. **Review Logs**: Check workflow execution logs
3. **Validate Configuration**: Ensure POM and Mule configs are correct
4. **Test Locally**: Run Maven commands locally first
5. **Check Anypoint Platform**: Verify environment and permissions

## 🔒 Security Best Practices

✅ **Secrets Management**
- Use GitHub Secrets for sensitive data
- Never commit credentials to code
- Rotate secrets regularly

✅ **Access Control**
- Use environment protection rules
- Require approvals for production deployments
- Implement branch protection

✅ **Monitoring**
- Enable workflow notifications
- Monitor deployment logs
- Set up alerts for failures

## 📈 Enhancement Opportunities

### Additional Integrations

- **Slack Notifications**: Add deployment status notifications
- **JIRA Integration**: Link deployments to tickets
- **Monitoring**: Integrate with APM tools
- **Security Scanning**: Add SAST/DAST tools

### Advanced Features

- **Blue-Green Deployments**: Zero-downtime deployments
- **Canary Releases**: Gradual rollouts
- **Rollback Automation**: Automatic rollback on failures
- **Performance Testing**: Automated performance validation

## 🆘 Support

For issues or questions:

1. Check workflow logs in GitHub Actions
2. Review this documentation
3. Consult MuleSoft documentation
4. Contact your DevOps team

---

**Created**: $(date)
**Version**: 1.0
**Maintainer**: DevOps Team