# Robot Framework Docker Action - Quick Reference

## Quick Start

### Basic Usage
```yaml
- name: Run Robot Framework Tests
  uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
```

### With Custom Configuration
```yaml
- name: Run Robot Framework Tests
  uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
  with:
    browser: 'firefox'
    robot_threads: 4
    robot_tests_dir: 'tests'
    robot_reports_dir: 'reports'
```

## Configuration Parameters

### Essential Parameters

| Parameter | Default | Description | Example |
|-----------|---------|-------------|---------|
| `browser` | `'chrome'` | Browser type | `'firefox'` |
| `robot_threads` | `1` | Parallel threads | `4` |
| `robot_tests_dir` | `'robot_tests'` | Test directory | `'tests'` |
| `robot_reports_dir` | `'reports'` | Reports directory | `'test_reports'` |

### Advanced Parameters

| Parameter | Default | Description | Example |
|-----------|---------|-------------|---------|
| `allowed_shared_memory` | `'1g'` | Shared memory | `'2g'` |
| `robot_options` | `''` | Robot options | `'--include smoke'` |
| `pabot_options` | `''` | Parallel options | `'--verbose'` |
| `robot_runner_image` | `'ppodgorsek/robot-framework:latest'` | Docker image | `'ppodgorsek/robot-framework:4.1.1'` |

## Common Use Cases

### 1. Smoke Tests Only
```yaml
- name: Run Smoke Tests
  uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
  with:
    robot_options: '--include smoke'
```

### 2. Parallel Execution
```yaml
- name: Run Tests in Parallel
  uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
  with:
    robot_threads: 4
    pabot_options: '--verbose --processes 4'
```

### 3. Custom Test Directory
```yaml
- name: Run Tests from Custom Directory
  uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
  with:
    robot_tests_dir: 'tests/automation'
    robot_resources_dir: 'resources'
```

### 4. Firefox Browser
```yaml
- name: Run Tests with Firefox
  uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
  with:
    browser: 'firefox'
    allowed_shared_memory: '2g'
```

### 5. Verbose Output
```yaml
- name: Run Tests with Verbose Output
  uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
  with:
    robot_options: '--verbose --loglevel DEBUG'
    pabot_options: '--verbose'
```

## Robot Framework Options

### Common Robot Options

| Option | Description | Example |
|--------|-------------|---------|
| `--include` | Run tests with specific tags | `--include smoke` |
| `--exclude` | Skip tests with specific tags | `--exclude slow` |
| `--outputdir` | Output directory | `--outputdir reports` |
| `--log` | Log file name | `--log log.html` |
| `--report` | Report file name | `--report report.html` |
| `--loglevel` | Log level | `--loglevel DEBUG` |
| `--variable` | Set variables | `--variable BROWSER:firefox` |

### Pabot Options (Parallel Execution)

| Option | Description | Example |
|--------|-------------|---------|
| `--processes` | Number of processes | `--processes 4` |
| `--verbose` | Verbose output | `--verbose` |
| `--artifacts` | Artifact directory | `--artifacts artifacts` |
| `--artifactsinsubfolders` | Organize artifacts | `--artifactsinsubfolders` |

## Troubleshooting

### Common Issues

#### 1. Browser Crashes
**Problem**: Browser crashes during test execution
**Solution**: Increase shared memory
```yaml
allowed_shared_memory: '2g'
```

#### 2. Permission Errors
**Problem**: Container cannot write to volumes
**Solution**: Check directory permissions (usually handled automatically)

#### 3. Tests Not Found
**Problem**: Robot Framework cannot find test files
**Solution**: Verify test directory path
```yaml
robot_tests_dir: 'tests/robot'
```

#### 4. Slow Test Execution
**Problem**: Tests run slowly
**Solution**: Use parallel execution
```yaml
robot_threads: 4
pabot_options: '--processes 4'
```

### Debug Commands

#### Enable Verbose Logging
```yaml
robot_options: '--verbose --loglevel DEBUG'
pabot_options: '--verbose'
```

#### Check Directory Structure
```bash
# Verify test directory exists
ls -la robot_tests/

# Verify resources directory exists
ls -la resources/
```

## Performance Optimization

### Memory Settings
```yaml
# For large test suites
allowed_shared_memory: '2g'

# For small test suites
allowed_shared_memory: '512m'
```

### Parallel Execution
```yaml
# Optimal for most cases
robot_threads: 4
pabot_options: '--processes 4'

# For very large test suites
robot_threads: 8
pabot_options: '--processes 8'
```

### Screen Configuration
```yaml
# Standard resolution
screen_width: 1920
screen_height: 1080

# Lower resolution for faster rendering
screen_width: 1366
screen_height: 768
```

## Security Best Practices

### Sensitive Data
```yaml
# Use GitHub Secrets for passwords
robot_user_password: ${{ secrets.ROBOT_PASSWORD }}

# Use GitHub Secrets for usernames
robot_user_login: ${{ secrets.ROBOT_USERNAME }}
```

### Docker Image Security
```yaml
# Pin specific version for stability
robot_runner_image: 'ppodgorsek/robot-framework:4.1.1'
```

## File Structure Examples

### Basic Structure
```
repository/
├── robot_tests/
│   ├── test1.robot
│   └── test2.robot
├── resources/
│   └── common.resource
├── reports/
└── .github/workflows/
    └── test.yml
```

### Advanced Structure
```
repository/
├── tests/
│   ├── smoke/
│   │   └── smoke_tests.robot
│   ├── regression/
│   │   └── regression_tests.robot
│   └── integration/
│       └── integration_tests.robot
├── resources/
│   ├── common.resource
│   ├── api.resource
│   └── ui.resource
├── test_reports/
└── .github/workflows/
    └── test.yml
```

## Environment Variables

### Available in Container
- `BROWSER`: Browser type (chrome/firefox)
- `ROBOT_THREADS`: Number of parallel threads
- `ROBOT_OPTIONS`: Robot Framework options
- `PABOT_OPTIONS`: Parallel execution options
- `ROBOT_USER_LOGIN`: Application login username
- `ROBOT_USER_PASSWORD`: Application login password

### Usage in Robot Tests
```robotframework
*** Variables ***
${BROWSER}    %{BROWSER}
${USER_LOGIN}    %{ROBOT_USER_LOGIN}
${USER_PASSWORD}    %{ROBOT_USER_PASSWORD}
```

## Complete Workflow Example

```yaml
name: Robot Framework Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      
      - name: Run Smoke Tests
        uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
        with:
          browser: 'chrome'
          robot_threads: 2
          robot_options: '--include smoke --outputdir reports'
          robot_tests_dir: 'tests'
          robot_reports_dir: 'reports'
          allowed_shared_memory: '1g'
          robot_user_login: ${{ secrets.TEST_USER }}
          robot_user_password: ${{ secrets.TEST_PASSWORD }}
      
      - name: Upload Reports
        uses: actions/upload-artifact@v2
        with:
          name: robot-reports
          path: reports/
```

## Support and Resources

### Documentation
- [API Documentation](API_DOCUMENTATION.md)
- [Component Documentation](COMPONENT_DOCUMENTATION.md)
- [Robot Framework Documentation](https://robotframework.org/robotframework/)

### Related Tools
- [ppodgorsek/robot-framework Docker Image](https://github.com/ppodgorsek/docker-robot-framework)
- [pabot Parallel Execution](https://github.com/mkorpela/pabot)
- [SeleniumLibrary](https://github.com/robotframework/SeleniumLibrary)