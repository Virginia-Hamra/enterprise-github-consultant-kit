# Test Generator Skill

## Purpose
Generates comprehensive, production-grade test suites for enterprise applications, infrastructure code, and GitHub Actions workflows. This skill supports **QA Engineers**, **Platform Engineers**, and **Developers** in establishing robust testing strategies for enterprise GitHub implementations.

## Role Context
Use this skill when building test automation for customer deliverables, validating CI/CD pipelines, or establishing quality gates for enterprise GitHub platforms.

## Capabilities

### Application Testing
- **Unit Tests**: Generate unit tests for functions, classes, and modules
- **Integration Tests**: Create tests for API endpoints, database operations, and service interactions
- **End-to-End Tests**: Build browser automation tests with Selenium, Playwright, Cypress
- **Contract Testing**: Generate Pact/OpenAPI contract tests for microservices
- **Performance Tests**: Create load tests with k6, JMeter, or Locust
- **Security Tests**: Generate OWASP ZAP, Burp Suite, or custom security test scenarios

### Infrastructure Testing
- **Terraform Tests**: Generate Terratest or terraform-compliance test suites
- **Kubernetes Tests**: Create kubectl, helm test, or Ginkgo-based validation tests
- **CloudFormation Tests**: Build cfn-lint, TaskCat, or custom validation tests
- **Ansible Tests**: Generate Molecule test scenarios for Ansible roles
- **Policy Tests**: Create OPA Rego test suites for policy-as-code validation
- **Compliance Tests**: Generate InSpec or custom compliance validation tests

### GitHub Actions Workflow Testing
- **Workflow Syntax Validation**: Generate tests to validate workflow YAML syntax
- **Action Testing**: Create tests for custom GitHub Actions (JavaScript, Docker, Composite)
- **Workflow Integration Tests**: Build end-to-end workflow execution tests
- **Security Testing**: Generate tests for workflow permission validation
- **Performance Testing**: Create tests to validate workflow execution time

### Test Data Generation
- **Fixture Creation**: Generate realistic test data for various scenarios
- **Mock Objects**: Create mocks, stubs, and fakes for external dependencies
- **Database Seeding**: Generate test database seed scripts
- **API Response Mocking**: Create mock server responses for API testing
- **File-Based Fixtures**: Generate JSON, YAML, CSV test data files

### Test Framework Support
- **JavaScript/TypeScript**: Jest, Mocha, Chai, Jasmine, Vitest
- **Python**: pytest, unittest, nose2, Robot Framework
- **Java**: JUnit, TestNG, Mockito, AssertJ
- **Go**: testing package, Testify, Ginkgo, Gomega
- **Ruby**: RSpec, Minitest
- **C#**: NUnit, xUnit, MSTest
- **Shell**: Bats, shunit2

## Usage Scenarios

### Scenario 1: Generate Unit Tests for Application Code
**Objective**: Create comprehensive unit test suite for existing codebase

```bash
# Generate unit tests for Python application
./scripts/generate-unit-tests.sh --language python --source src/ --output tests/unit/

# Generate unit tests for TypeScript application
./scripts/generate-unit-tests.sh --language typescript --source src/ --output tests/unit/ --framework jest
```

**Generated Output**:
- Test files mirroring source structure
- Mocks for external dependencies
- Test data fixtures
- Coverage configuration
- CI integration instructions

### Scenario 2: Create Infrastructure Tests
**Objective**: Generate Terraform validation tests for infrastructure code

```bash
# Generate Terratest suite
./scripts/generate-terraform-tests.sh --modules terraform/modules/ --output tests/terraform/

# Generate terraform-compliance policies
./scripts/generate-terraform-policies.sh --modules terraform/modules/ --framework terraform-compliance
```

**Test Types**:
- Resource creation validation
- Security configuration checks
- Cost estimation validation
- Tag enforcement tests
- Network configuration validation

### Scenario 3: Build GitHub Actions Workflow Tests
**Objective**: Create tests for custom GitHub Actions and workflows

```bash
# Generate tests for custom action
./scripts/generate-action-tests.sh --action .github/actions/my-action --framework jest

# Generate workflow integration tests
./scripts/generate-workflow-tests.sh --workflows .github/workflows/ --output .github/tests/
```

**Test Coverage**:
- Action inputs and outputs validation
- Environment variable handling
- Error scenarios and exit codes
- Permissions requirements
- Integration with GitHub APIs

### Scenario 4: Create API Integration Tests
**Objective**: Generate comprehensive API test suite with contract testing

```bash
# Generate REST API tests
./scripts/generate-api-tests.sh --openapi spec/openapi.yaml --output tests/integration/ --framework pytest

# Generate contract tests
./scripts/generate-contract-tests.sh --provider api-service --consumers web-app,mobile-app --framework pact
```

**Test Scenarios**:
- Happy path requests
- Error handling (4xx, 5xx responses)
- Authentication and authorization
- Rate limiting behavior
- Pagination and filtering
- Request validation
- Response schema validation

### Scenario 5: Generate Security Tests
**Objective**: Create security test suite for web application

```bash
# Generate OWASP Top 10 tests
./scripts/generate-security-tests.sh --app-url https://staging.example.com --output tests/security/

# Generate dependency vulnerability tests
./scripts/generate-dependency-tests.sh --manifest package.json --output tests/security/dependencies/
```

**Security Test Categories**:
- SQL Injection
- Cross-Site Scripting (XSS)
- CSRF protection
- Authentication bypass attempts
- Authorization boundary tests
- Sensitive data exposure
- Security headers validation
- Dependency vulnerability checks

## Scripts

### `scripts/generate-unit-tests.sh`
Generates unit test suites for application code.

**Usage**:
```bash
./generate-unit-tests.sh --language <lang> --source <dir> --output <dir> [--framework <name>] [--coverage-threshold N]
```

**Supported Languages**: python, typescript, javascript, java, go, ruby, csharp

**Options**:
- `--language`: Programming language (required)
- `--source`: Source code directory (required)
- `--output`: Test output directory (required)
- `--framework`: Testing framework (optional, auto-detects if not specified)
- `--coverage-threshold`: Minimum coverage percentage (default: 80)
- `--mock-external`: Generate mocks for external dependencies

**Output Structure**:
```
tests/unit/
├── conftest.py (pytest) or setup.js (jest)
├── fixtures/
│   └── test-data.json
├── mocks/
│   └── external-service-mock.py
└── test_module_name.py (mirrors source structure)
```

### `scripts/generate-terraform-tests.sh`
Generates Terratest or terraform-compliance tests for Terraform modules.

**Usage**:
```bash
./generate-terraform-tests.sh --modules <dir> --output <dir> [--framework terratest|terraform-compliance]
```

**Test Categories**:
- Resource existence validation
- Configuration correctness checks
- Security best practices
- Naming convention enforcement
- Tag requirements validation

### `scripts/generate-action-tests.sh`
Creates test suite for custom GitHub Actions.

**Usage**:
```bash
./generate-action-tests.sh --action <path> --framework <jest|bats> [--integration]
```

**Test Types**:
- Unit tests for action logic
- Input validation tests
- Output verification tests
- Error handling tests
- Integration tests with GitHub APIs (if --integration flag)

### `scripts/generate-workflow-tests.sh`
Generates validation tests for GitHub Actions workflows.

**Usage**:
```bash
./generate-workflow-tests.sh --workflows <dir> --output <dir>
```

**Validations**:
- YAML syntax correctness
- Required permissions declared
- Secret references valid
- Action versions pinned
- Timeout limits set
- Required status checks configured

### `scripts/generate-api-tests.sh`
Creates API test suite from OpenAPI/Swagger specifications.

**Usage**:
```bash
./generate-api-tests.sh --openapi <file> --output <dir> --framework <pytest|jest|postman>
```

**Features**:
- Generates tests for all endpoints
- Includes positive and negative test cases
- Creates authentication helpers
- Generates request/response validators
- Includes data-driven test scenarios

### `scripts/generate-contract-tests.sh`
Generates consumer and provider contract tests.

**Usage**:
```bash
./generate-contract-tests.sh --provider <name> --consumers <list> --framework <pact|spring-cloud-contract>
```

**Output**: Consumer-driven contract tests for all specified consumers

### `scripts/generate-security-tests.sh`
Creates security test suite covering OWASP Top 10 and common vulnerabilities.

**Usage**:
```bash
./generate-security-tests.sh --app-url <url> --output <dir> [--auth-token <token>]
```

**Test Categories**:
- Injection attacks (SQL, Command, LDAP)
- Broken authentication
- Sensitive data exposure
- XML External Entities (XXE)
- Broken access control
- Security misconfiguration
- Cross-Site Scripting (XSS)
- Insecure deserialization
- Using components with known vulnerabilities
- Insufficient logging & monitoring

### `scripts/generate-load-tests.sh`
Generates performance and load test scripts.

**Usage**:
```bash
./generate-load-tests.sh --app-url <url> --scenarios <file> --framework <k6|jmeter|locust>
```

**Test Scenarios**:
- Baseline load testing
- Stress testing (find breaking point)
- Spike testing (sudden load increases)
- Soak testing (sustained load)
- Scalability testing

### `scripts/generate-test-data.sh`
Creates realistic test data fixtures.

**Usage**:
```bash
./generate-test-data.sh --schema <file> --count <N> --format <json|csv|sql> --output <file>
```

**Features**:
- Generates realistic data based on schema
- Supports relationships between entities
- Creates valid and invalid data scenarios
- Handles localization requirements

## Test Strategy Templates

### Enterprise Application Test Strategy

```yaml
# test-strategy.yaml
test_pyramid:
  unit_tests:
    coverage_threshold: 80%
    frameworks: [pytest, jest]
    execution: "on every commit"

  integration_tests:
    coverage_threshold: 60%
    frameworks: [pytest, supertest]
    execution: "on pull request"

  e2e_tests:
    coverage_threshold: 40%
    frameworks: [playwright, cypress]
    execution: "on main branch, nightly"

  performance_tests:
    frameworks: [k6]
    execution: "weekly, on main branch"

  security_tests:
    frameworks: [zap, snyk]
    execution: "on pull request, nightly"

quality_gates:
  - type: coverage
    threshold: 80%
    blocking: true

  - type: security
    severity: high
    blocking: true

  - type: performance
    p95_response_time: 500ms
    blocking: false

  - type: flaky_tests
    max_flaky_rate: 2%
    blocking: true
```

### Infrastructure Test Strategy

```yaml
# infra-test-strategy.yaml
test_layers:
  syntax_validation:
    tools: [terraform validate, cfn-lint, yamllint]
    execution: "pre-commit hook"

  policy_validation:
    tools: [OPA, terraform-compliance, Checkov]
    execution: "on pull request"

  integration_tests:
    tools: [Terratest, Kitchen-Terraform]
    execution: "on pull request (ephemeral environment)"

  compliance_tests:
    tools: [InSpec, Scout Suite]
    execution: "post-deployment"

  cost_validation:
    tools: [Infracost, custom scripts]
    execution: "on pull request"
```

## Best Practices

### When Generating Tests
- **Start with happy paths**, then add edge cases and error scenarios
- **Follow AAA pattern**: Arrange, Act, Assert for clarity
- **Keep tests independent**: Each test should run in isolation
- **Use descriptive names**: Test names should describe what is being tested and expected outcome
- **Don't test implementation details**: Test behavior, not internal structure
- **Maintain test data separately**: Use fixtures, factories, or builders

### Test Organization
- **Mirror source structure**: Place tests adjacent to or mirroring source files
- **Group related tests**: Use test suites or describe blocks for logical grouping
- **Tag tests appropriately**: Use markers/tags for smoke, regression, slow tests
- **Separate unit from integration**: Different directories and execution strategies
- **Version test data**: Track changes to fixtures and test databases

### CI/CD Integration
- **Run fast tests first**: Unit tests before integration tests
- **Parallelize test execution**: Use matrix strategies or test parallelization tools
- **Fail fast on critical failures**: Don't waste time if build fails
- **Report test results clearly**: Use test report Actions or tools
- **Track flaky tests**: Monitor and fix unstable tests promptly
- **Archive test artifacts**: Save logs, screenshots, and reports

### Test Maintenance
- **Remove obsolete tests**: Delete tests for removed features
- **Refactor duplicated code**: Use helper functions and shared fixtures
- **Update tests with code changes**: Keep tests synchronized with implementation
- **Review test coverage regularly**: Identify gaps and redundancy
- **Monitor test execution time**: Optimize slow tests

## Deliverables Template

When delivering a test suite, provide:

1. **Test Strategy Document**
   - Test pyramid breakdown
   - Coverage targets per layer
   - Testing frameworks and rationale
   - Execution triggers and frequency
   - Quality gates and blocking criteria

2. **Test Suite Structure**
   - Organized test files and directories
   - Shared utilities and helpers
   - Test data fixtures and factories
   - Configuration files
   - README with setup and execution instructions

3. **CI/CD Integration**
   - GitHub Actions workflows for test execution
   - Test result reporting configuration
   - Coverage reporting setup
   - Quality gate enforcement
   - Test environment provisioning

4. **Documentation**
   - How to run tests locally
   - How to add new tests
   - Troubleshooting common issues
   - Test data management
   - Debugging failed tests

5. **Quality Metrics Dashboard**
   - Test coverage trends
   - Test execution time trends
   - Flaky test tracking
   - Failure rate monitoring
   - Quality gate compliance

## Integration with Other Skills

- **DevOps Review**: Tests validate CI/CD pipeline correctness
- **Architecture Analysis**: Test strategy follows architecture decisions
- **Security Audit**: Security tests validate security controls
- **Migration Planner**: Tests verify migration success criteria

## Customer Engagement Tips

### Discovery Questions
- "What is your current test coverage?"
- "What types of bugs escape to production most often?"
- "How long do your test suites take to run?"
- "What testing frameworks are your teams most comfortable with?"
- "What quality gates are required for production deployments?"

### Common Challenges
- **"We don't have time to write tests"**: Show ROI of catching bugs early
- **"Tests are flaky and unreliable"**: Demonstrate strategies to stabilize tests
- **"Tests are too slow"**: Identify optimization opportunities and parallelization
- **"We don't know what to test"**: Guide on risk-based testing prioritization

### Success Metrics
- Reduction in production incidents
- Faster feedback on pull requests
- Increased developer confidence
- Reduced time debugging issues
- Improved code coverage over time

## Reference Materials

- [Test Pyramid - Martin Fowler](https://martinfowler.com/articles/practical-test-pyramid.html)
- [Testing GitHub Actions - GitHub Docs](https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action#testing-out-your-action)
- [Terratest Documentation](https://terratest.gruntwork.io/)
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [Contract Testing - Pact](https://docs.pact.io/)
