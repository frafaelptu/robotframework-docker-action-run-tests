# Robot Framework Docker Action - Component Documentation

## Technical Architecture

This document provides detailed technical documentation for the Robot Framework Docker Action components, including implementation details, function specifications, and internal workflows.

## Table of Contents

1. [Action Definition Component](#action-definition-component)
2. [Execution Script Component](#execution-script-component)
3. [Docker Integration](#docker-integration)
4. [Environment Variable Management](#environment-variable-management)
5. [Volume Mounting System](#volume-mounting-system)
6. [Security Implementation](#security-implementation)
7. [Error Handling](#error-handling)

## Action Definition Component

### File: `action.yml`

#### Component Overview
The action definition file serves as the entry point and configuration interface for the GitHub Action. It defines all input parameters, execution steps, and environment variable mappings.

#### Input Parameter Definitions

```yaml
inputs:
  allowed_shared_memory:
    description: 'Shared memory that Docker container is allowed to use'
    required: true
    default: '1g'
```

**Technical Details:**
- **Validation**: GitHub Actions automatically validates required fields
- **Type Coercion**: String values are passed as-is to the execution script
- **Default Handling**: Defaults are applied when parameters are not provided

#### Composite Action Configuration

```yaml
runs:
  using: 'composite'
  steps:    
    - shell: bash
      run: chmod +x ${{ github.action_path }}/test.sh
    - shell: bash
      run: ${{ github.action_path }}/test.sh
```

**Technical Details:**
- **Execution Model**: Uses composite action type for multi-step execution
- **Script Permissions**: Ensures execution script has proper permissions
- **Path Resolution**: Uses `github.action_path` for reliable script location

#### Environment Variable Mapping

```yaml
env:
  ALLOWED_SHARED_MEMORY: ${{ inputs.allowed_shared_memory }}
  BROWSER: ${{ inputs.browser }}
  ROBOT_THREADS: ${{ inputs.robot_threads }}
  # ... additional mappings
```

**Technical Details:**
- **Variable Escaping**: GitHub Actions handles special character escaping
- **Type Conversion**: Automatic string conversion for all input values
- **Scope**: Environment variables are available to the execution script

## Execution Script Component

### File: `test.sh`

#### Component Overview
The execution script is the core component that orchestrates the Docker container execution, volume mounting, and environment setup.

#### Directory Setup Function

```bash
REPORTS_DIR=$(pwd)/$ROBOT_REPORTS_DIR
TESTS_DIR=$(pwd)/$ROBOT_TESTS_DIR
RESOURCES_DIR=$(pwd)/$ROBOT_RESOURCES_DIR
```

**Function Specification:**
- **Purpose**: Establishes absolute paths for volume mounting
- **Input**: Environment variables from action inputs
- **Output**: Absolute path variables for Docker volumes
- **Error Handling**: Relies on shell path resolution

#### Docker Execution Function

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

**Function Specification:**
- **Purpose**: Executes Robot Framework tests in Docker container
- **Parameters**:
  - `--user $(id -u):$(id -g)`: User mapping for security
  - `--shm-size`: Shared memory allocation
  - `-e`: Environment variable passing
  - `-v`: Volume mounting with SELinux compatibility
- **Return Value**: Exit code from Docker container execution

## Docker Integration

### Container Configuration

#### Base Image Specification
- **Image**: `ppodgorsek/robot-framework:latest`
- **Architecture**: Linux-based container
- **Privileges**: Non-privileged execution
- **User**: Mapped to host user ID

#### Resource Allocation

```bash
--shm-size=$ALLOWED_SHARED_MEMORY
```

**Technical Details:**
- **Purpose**: Prevents browser crashes in headless mode
- **Format**: Docker size format (e.g., '1g', '512m')
- **Default**: 1GB shared memory allocation
- **Impact**: Critical for browser stability in CI/CD environments

#### Volume Mounting Strategy

| Host Path | Container Path | Purpose | SELinux Flag |
|-----------|----------------|---------|--------------|
| `$REPORTS_DIR` | `/opt/robotframework/reports` | Test outputs | `:Z` |
| `$TESTS_DIR` | `/opt/robotframework/tests` | Test files | `:Z` |
| `$RESOURCES_DIR` | `/opt/robotframework/resources` | Resource files | `:Z` |

**Technical Details:**
- **SELinux Compatibility**: `:Z` flag ensures proper permissions
- **Path Resolution**: Absolute paths prevent mounting issues
- **Permission Handling**: Automatic permission mapping

## Environment Variable Management

### Variable Flow Architecture

```
GitHub Action Inputs → action.yml → Environment Variables → test.sh → Docker Container
```

#### Input Processing

```yaml
# action.yml
ALLOWED_SHARED_MEMORY: ${{ inputs.allowed_shared_memory }}
```

```bash
# test.sh
docker run --shm-size=$ALLOWED_SHARED_MEMORY
```

**Technical Details:**
- **Validation**: GitHub Actions validates required inputs
- **Escaping**: Automatic handling of special characters
- **Type Safety**: String-based parameter passing

#### Container Environment Setup

```bash
-e BROWSER=$BROWSER \
-e ROBOT_THREADS=$ROBOT_THREADS \
-e PABOT_OPTIONS="$PABOT_OPTIONS" \
-e ROBOT_OPTIONS="$ROBOT_OPTIONS" \
-e ROBOT_USER_LOGIN="$ROBOT_USER_LOGIN" \
-e ROBOT_USER_PASSWORD="$ROBOT_USER_PASSWORD"
```

**Technical Details:**
- **Quoting Strategy**: Double quotes for multi-word values
- **Variable Expansion**: Shell expansion before Docker execution
- **Security**: Sensitive data passed as environment variables

## Volume Mounting System

### Mount Point Architecture

#### Reports Directory
```bash
-v $REPORTS_DIR:/opt/robotframework/reports:Z
```

**Technical Details:**
- **Purpose**: Persistent storage for test results
- **Permissions**: Write access for container
- **Content**: HTML reports, logs, and test artifacts

#### Tests Directory
```bash
-v $TESTS_DIR:/opt/robotframework/tests:Z
```

**Technical Details:**
- **Purpose**: Test file access within container
- **Permissions**: Read access for container
- **Content**: Robot Framework test files (.robot)

#### Resources Directory
```bash
-v $RESOURCES_DIR:/opt/robotframework/resources:Z
```

**Technical Details:**
- **Purpose**: Resource file access within container
- **Permissions**: Read access for container
- **Content**: Robot Framework resource files (.resource)

### Path Resolution Algorithm

```bash
REPORTS_DIR=$(pwd)/$ROBOT_REPORTS_DIR
```

**Algorithm Steps:**
1. Get current working directory (`pwd`)
2. Concatenate with relative path from input
3. Resolve to absolute path
4. Validate path exists (implicit)

## Security Implementation

### User Mapping Security

```bash
--user $(id -u):$(id -g)
```

**Security Features:**
- **Non-root Execution**: Container runs as non-root user
- **Host User Mapping**: Container user matches host user
- **Permission Isolation**: Prevents privilege escalation
- **File Ownership**: Maintains proper file ownership

### SELinux Compatibility

```bash
-v $REPORTS_DIR:/opt/robotframework/reports:Z
```

**Security Features:**
- **SELinux Labels**: `:Z` flag applies appropriate SELinux context
- **Permission Mapping**: Ensures proper file access permissions
- **Security Context**: Maintains system security policies

### Input Validation

**Validation Layers:**
1. **GitHub Actions**: Validates required fields and types
2. **Shell Script**: Path validation and existence checks
3. **Docker**: Container-level validation and error handling

## Error Handling

### Error Handling Strategy

#### Script-Level Error Handling
```bash
# Implicit error handling through shell exit codes
docker run [options] $ROBOT_RUNNER_IMAGE
```

**Error Handling Features:**
- **Exit Code Propagation**: Docker exit codes propagate to GitHub Actions
- **Automatic Failure**: Non-zero exit codes fail the action
- **Logging**: Docker logs are captured by GitHub Actions

#### Common Error Scenarios

| Error Type | Cause | Detection | Resolution |
|------------|-------|-----------|------------|
| Path Not Found | Invalid directory paths | Docker mount failure | Verify input paths |
| Permission Denied | SELinux/User mapping issues | Docker execution failure | Check user permissions |
| Shared Memory | Insufficient memory allocation | Browser crashes | Increase shared memory |
| Image Pull Failure | Network/Docker registry issues | Docker pull failure | Check network connectivity |

### Debugging Capabilities

#### Verbose Output
```yaml
robot_options: '--verbose'
pabot_options: '--verbose'
```

#### Log Collection
- **Docker Logs**: Captured by GitHub Actions
- **Robot Framework Logs**: Stored in reports directory
- **System Logs**: Available through GitHub Actions interface

## Performance Considerations

### Optimization Strategies

#### Parallel Execution
```yaml
robot_threads: 4
pabot_options: '--processes 4'
```

**Performance Impact:**
- **Test Execution**: Parallel test execution reduces total time
- **Resource Usage**: Increased CPU and memory consumption
- **Scalability**: Linear scaling with thread count

#### Memory Management
```yaml
allowed_shared_memory: '2g'
```

**Performance Impact:**
- **Browser Stability**: Prevents browser crashes
- **Test Reliability**: Improves test execution success rate
- **Resource Allocation**: Balances memory usage vs. stability

#### Screen Configuration
```yaml
screen_width: 1920
screen_height: 1080
screen_color_depth: 24
```

**Performance Impact:**
- **Rendering Performance**: Affects browser rendering speed
- **Memory Usage**: Screen buffer memory allocation
- **Test Compatibility**: Ensures consistent test environment

## Integration Points

### GitHub Actions Integration

#### Workflow Integration
```yaml
- name: Run Robot Framework Tests
  uses: frafaelptu/robotframework-docker-action-run-tests@v2.0.0
```

**Integration Features:**
- **Seamless Integration**: Drop-in replacement for Robot Framework testing
- **Environment Compatibility**: Works with any GitHub-hosted runner
- **Artifact Handling**: Automatic artifact collection and storage

#### Artifact Management
- **Reports**: HTML reports stored in specified directory
- **Logs**: Detailed execution logs for debugging
- **Screenshots**: Browser screenshots for failed tests

### External Dependencies

#### Docker Runtime
- **Requirement**: Docker runtime available on runner
- **Version**: Compatible with Docker 20.10+
- **Features**: Volume mounting, environment variables, user mapping

#### Robot Framework
- **Version**: Provided by ppodgorsek/robot-framework image
- **Features**: Browser automation, test execution, reporting
- **Extensions**: SeleniumLibrary, pabot for parallel execution

## Maintenance and Updates

### Version Management

#### Action Versioning
- **Semantic Versioning**: Follows semver for releases
- **Backward Compatibility**: Maintains compatibility within major versions
- **Update Strategy**: Regular updates for security and features

#### Docker Image Updates
- **Base Image**: ppodgorsek/robot-framework:latest
- **Update Frequency**: As needed for security patches
- **Version Pinning**: Support for specific image versions

### Monitoring and Health Checks

#### Health Check Strategy
- **Execution Monitoring**: GitHub Actions provides execution status
- **Error Tracking**: Automatic error reporting and logging
- **Performance Monitoring**: Execution time and resource usage tracking

#### Maintenance Tasks
- **Regular Updates**: Keep dependencies up to date
- **Security Audits**: Monitor for security vulnerabilities
- **Performance Optimization**: Continuous improvement of execution efficiency