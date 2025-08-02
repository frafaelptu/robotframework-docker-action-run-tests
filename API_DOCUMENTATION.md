# Robot Framework Docker Action - API Documentation

## Overview

The Robot Framework Docker Action is a GitHub Action that enables automated testing using Robot Framework within a Docker container. This action provides a seamless way to run browser-based automated tests in CI/CD pipelines using the [ppodgorsek/robot-framework](https://github.com/ppodgorsek/docker-robot-framework) Docker image.

## Table of Contents

1. [Action Configuration](#action-configuration)
2. [Input Parameters](#input-parameters)
3. [Usage Examples](#usage-examples)
4. [Environment Variables](#environment-variables)
5. [Docker Configuration](#docker-configuration)
6. [File Structure](#file-structure)
7. [Troubleshooting](#troubleshooting)

## Action Configuration

### Action Name
```yaml
frafaelptu/robotframework-docker-action-run-tests@v2.0.0
```

### Action Type
- **Type**: Composite Action
- **Platform**: Linux (Ubuntu)
- **Docker-based**: Yes

## Input Parameters

### Core Configuration

#### `browser`
- **Type**: String
- **Required**: Yes
- **Default**: `'chrome'`
- **Description**: Specifies the browser to use for test execution
- **Valid Values**: `'chrome'`, `'firefox'`
- **Example**:
  ```yaml
  browser: 'firefox'
  ```

#### `robot_threads`
- **Type**: Integer
- **Required**: Yes
- **Default**: `1`
- **Description**: Number of parallel threads for test execution. Use values > 1 for parallel testing
- **Range**: 1 - ∞
- **Example**:
  ```yaml
  robot_threads: 4
  ```

#### `robot_tests_dir`
- **Type**: String
- **Required**: Yes
- **Default**: `'robot_tests'`
- **Description**: Directory path where Robot Framework test files are located in the repository
- **Example**:
  ```yaml
  robot_tests_dir: 'tests/automation'
  ```

#### `robot_resources_dir`
- **Type**: String
- **Required**: Yes
- **Default**: `'resources'`
- **Description**: Directory path where Robot Framework resource files are located in the repository
- **Example**:
  ```yaml
  robot_resources_dir: 'test_resources'
  ```

#### `robot_reports_dir`
- **Type**: String
- **Required**: Yes
- **Default**: `'reports'`
- **Description**: Directory where test execution reports will be saved
- **Example**:
  ```yaml
  robot_reports_dir: 'test_reports'
  ```

### Docker Configuration

#### `allowed_shared_memory`
- **Type**: String
- **Required**: Yes
- **Default**: `'1g'`
- **Description**: Shared memory allocation for the Docker container
- **Format**: Size with unit (e.g., '512m', '2g')
- **Example**:
  ```yaml
  allowed_shared_memory: '2g'
  ```

#### `robot_runner_image`
- **Type**: String
- **Required**: Yes
- **Default**: `'ppodgorsek/robot-framework:latest'`
- **Description**: Docker image to use for test execution
- **Example**:
  ```yaml
  robot_runner_image: 'ppodgorsek/robot-framework:4.1.1'
  ```

### Display Configuration

#### `screen_width`
- **Type**: Integer
- **Required**: Yes
- **Default**: `1920`
- **Description**: Width of the virtual screen in pixels
- **Example**:
  ```yaml
  screen_width: 1366
  ```

#### `screen_height`
- **Type**: Integer
- **Required**: Yes
- **Default**: `1080`
- **Description**: Height of the virtual screen in pixels
- **Example**:
  ```yaml
  screen_height: 768
  ```

#### `screen_color_depth`
- **Type**: Integer
- **Required**: Yes
- **Default**: `24`
- **Description**: Color depth of the virtual screen in bits
- **Example**:
  ```yaml
  screen_color_depth: 16
  ```

### Test Execution Options

#### `robot_options`
- **Type**: String
- **Required**: Yes
- **Default**: `''`
- **Description**: Additional command-line options for Robot Framework execution
- **Example**:
  ```yaml
  robot_options: '--include smoke --exclude slow'
  ```

#### `pabot_options`
- **Type**: String
- **Required**: Yes
- **Default**: `''`
- **Description**: Additional options for parallel execution (only used when robot_threads > 1)
- **Example**:
  ```yaml
  pabot_options: '--verbose --processes 4'
  ```

### Authentication Configuration

#### `robot_user_login`
- **Type**: String
- **Required**: Yes
- **Default**: `'fulano@mail.com.br'`
- **Description**: Username/email for application login during tests
- **Example**:
  ```yaml
  robot_user_login: 'testuser@example.com'
  ```

#### `robot_user_password`
- **Type**: String
- **Required**: Yes
- **Default**: `'123456'`
- **Description**: Password for application login during tests
- **Example**:
  ```yaml
  robot_user_password: 'secure_password_123'
  ```

## Usage Examples

### Basic Usage with Chrome

```yaml
name: Run Robot Framework Tests
on: [push, pull_request]

jobs:
  robot_test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      
      - name: Run Robot Framework Tests
        uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
```

### Advanced Configuration with Firefox and Parallel Execution

```yaml
name: Advanced Robot Framework Tests
on: [push, pull_request]

jobs:
  robot_test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      
      - name: Run Robot Framework Tests
        uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
        with:
          browser: 'firefox'
          robot_threads: 4
          robot_options: '--include smoke --exclude slow'
          pabot_options: '--verbose --processes 4'
          robot_tests_dir: 'tests/automation'
          robot_resources_dir: 'test_resources'
          robot_reports_dir: 'test_reports'
          allowed_shared_memory: '2g'
          screen_width: 1366
          screen_height: 768
          robot_user_login: 'testuser@example.com'
          robot_user_password: 'secure_password_123'
```

### Custom Docker Image Usage

```yaml
name: Custom Robot Framework Tests
on: [push, pull_request]

jobs:
  robot_test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      
      - name: Run Robot Framework Tests
        uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
        with:
          robot_runner_image: 'ppodgorsek/robot-framework:4.1.1'
          robot_threads: 2
          robot_options: '--outputdir reports --log log.html --report report.html'
```

## Environment Variables

The action sets the following environment variables within the Docker container:

| Variable | Description | Source |
|----------|-------------|---------|
| `BROWSER` | Browser type for test execution | `inputs.browser` |
| `ROBOT_THREADS` | Number of parallel threads | `inputs.robot_threads` |
| `PABOT_OPTIONS` | Parallel execution options | `inputs.pabot_options` |
| `ROBOT_OPTIONS` | Robot Framework options | `inputs.robot_options` |
| `ROBOT_USER_LOGIN` | Application login username | `inputs.robot_user_login` |
| `ROBOT_USER_PASSWORD` | Application login password | `inputs.robot_user_password` |

## Docker Configuration

### Container Setup

The action uses the following Docker run command structure:

```bash
docker run --user $(id -u):$(id -g) --shm-size=$ALLOWED_SHARED_MEMORY \
  -e BROWSER=$BROWSER \
  -e ROBOT_THREADS=$ROBOT_THREADS \
  -e PABOT_OPTIONS="$PABOT_OPTIONS" \
  -e ROBOT_OPTIONS="$ROBOT_OPTIONS" \
  -e ROBOT_USER_LOGIN="$ROBOT_USER_LOGIN" \
  -e ROBOT_USER_PASSWORD="$ROBOT_USER_PASSWORD" \
  -v $REPORTS_DIR:/opt/robotframework/reports:Z \
  -v $TESTS_DIR:/opt/robotframework/tests:Z \
  -v $RESOURCES_DIR:/opt/robotframework/resources:Z \
  $ROBOT_RUNNER_IMAGE
```

### Volume Mounts

| Host Path | Container Path | Purpose |
|-----------|----------------|---------|
| `$REPORTS_DIR` | `/opt/robotframework/reports` | Test execution reports |
| `$TESTS_DIR` | `/opt/robotframework/tests` | Robot Framework test files |
| `$RESOURCES_DIR` | `/opt/robotframework/resources` | Robot Framework resource files |

### Security Features

- **User Mapping**: Uses `--user $(id -u):$(id -g)` to run container as current user
- **Shared Memory**: Configurable shared memory allocation for browser stability
- **Volume Permissions**: Uses `:Z` flag for SELinux compatibility

## File Structure

```
.
├── action.yml              # Action definition and input parameters
├── test.sh                 # Main execution script
├── README.md               # Basic usage documentation
└── API_DOCUMENTATION.md    # This comprehensive documentation
```

### Action Definition (`action.yml`)

The action definition file contains:
- Input parameter definitions with types, descriptions, and defaults
- Composite action configuration
- Environment variable mapping
- Step execution configuration

### Execution Script (`test.sh`)

The main execution script performs:
- Directory setup and validation
- Docker container execution
- Volume mounting configuration
- Environment variable passing

## Troubleshooting

### Common Issues

#### 1. Shared Memory Errors
**Problem**: Browser crashes or tests fail due to insufficient shared memory
**Solution**: Increase `allowed_shared_memory` value
```yaml
allowed_shared_memory: '2g'
```

#### 2. Permission Errors
**Problem**: Container cannot write to mounted volumes
**Solution**: Ensure proper user mapping (handled automatically by the action)

#### 3. Browser Launch Failures
**Problem**: Browser fails to start in container
**Solution**: Check screen configuration and shared memory settings
```yaml
screen_width: 1366
screen_height: 768
allowed_shared_memory: '1g'
```

#### 4. Test File Not Found
**Problem**: Robot Framework cannot find test files
**Solution**: Verify `robot_tests_dir` path and file structure
```yaml
robot_tests_dir: 'tests/robot'
```

### Debug Mode

To enable verbose output, use:
```yaml
robot_options: '--verbose'
pabot_options: '--verbose'
```

### Performance Optimization

For faster test execution:
1. Use parallel execution: `robot_threads: 4`
2. Increase shared memory: `allowed_shared_memory: '2g'`
3. Use specific Robot Framework options for your use case

## Best Practices

1. **Security**: Use GitHub Secrets for sensitive data like passwords
2. **Performance**: Use parallel execution for large test suites
3. **Maintenance**: Pin specific Docker image versions for stability
4. **Organization**: Structure test files in logical directories
5. **Reporting**: Configure appropriate report directories and formats

## Related Resources

- [Robot Framework Documentation](https://robotframework.org/robotframework/)
- [ppodgorsek/robot-framework Docker Image](https://github.com/ppodgorsek/docker-robot-framework)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [pabot Parallel Execution](https://github.com/mkorpela/pabot)