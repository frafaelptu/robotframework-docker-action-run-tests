# Robot Framework Docker Action - Documentation Index

## Overview

This repository contains comprehensive documentation for the Robot Framework Docker Action, a GitHub Action that enables automated testing using Robot Framework within a Docker container. This documentation suite provides complete coverage of all public APIs, functions, components, and usage patterns.

## Documentation Structure

### 📚 [API Documentation](API_DOCUMENTATION.md)
**Complete API reference with detailed parameter specifications**

- **Action Configuration**: Action name, type, and platform details
- **Input Parameters**: All 13 configurable parameters with types, defaults, and examples
- **Usage Examples**: Basic, advanced, and custom configuration examples
- **Environment Variables**: Complete mapping of environment variables
- **Docker Configuration**: Container setup, volume mounts, and security features
- **Troubleshooting**: Common issues and solutions
- **Best Practices**: Security, performance, and maintenance guidelines

**Best for**: Developers who need complete API reference and detailed parameter documentation

### 🔧 [Component Documentation](COMPONENT_DOCUMENTATION.md)
**Technical implementation details and internal architecture**

- **Technical Architecture**: Detailed component breakdown and workflows
- **Action Definition Component**: `action.yml` file analysis and configuration
- **Execution Script Component**: `test.sh` script functions and specifications
- **Docker Integration**: Container configuration and resource allocation
- **Environment Variable Management**: Variable flow and processing
- **Volume Mounting System**: Mount point architecture and path resolution
- **Security Implementation**: User mapping, SELinux compatibility, and validation
- **Error Handling**: Error scenarios, debugging, and resolution strategies
- **Performance Considerations**: Optimization strategies and impact analysis

**Best for**: Developers who need to understand the internal workings, troubleshoot issues, or contribute to the project

### ⚡ [Quick Reference](QUICK_REFERENCE.md)
**Fast access to common usage patterns and troubleshooting**

- **Quick Start**: Basic usage examples and common configurations
- **Configuration Parameters**: Essential and advanced parameters at a glance
- **Common Use Cases**: 5 practical examples for different scenarios
- **Robot Framework Options**: Common options and parallel execution settings
- **Troubleshooting**: Quick fixes for common issues
- **Performance Optimization**: Memory, parallel execution, and screen settings
- **Security Best Practices**: Sensitive data handling and Docker security
- **File Structure Examples**: Basic and advanced project structures
- **Complete Workflow Example**: End-to-end GitHub Actions workflow

**Best for**: Users who need quick answers, common patterns, or troubleshooting help

### 📖 [README.md](README.md)
**Basic usage documentation and examples**

- **Example Usage**: Basic and advanced workflow examples
- **Configuration Table**: Parameter reference with defaults and descriptions
- **Simple Setup**: Quick start guide for basic usage

**Best for**: Users who want a quick overview and basic examples

## Documentation Navigation Guide

### 🚀 Getting Started
1. **New Users**: Start with [README.md](README.md) for basic examples
2. **Quick Setup**: Use [Quick Reference](QUICK_REFERENCE.md) for common patterns
3. **Detailed Configuration**: Refer to [API Documentation](API_DOCUMENTATION.md) for complete parameter details

### 🔍 Finding Specific Information

#### Configuration and Parameters
- **All Parameters**: [API Documentation - Input Parameters](API_DOCUMENTATION.md#input-parameters)
- **Quick Parameter Reference**: [Quick Reference - Configuration Parameters](QUICK_REFERENCE.md#configuration-parameters)
- **Technical Details**: [Component Documentation - Input Parameter Definitions](COMPONENT_DOCUMENTATION.md#input-parameter-definitions)

#### Usage Examples
- **Basic Examples**: [API Documentation - Usage Examples](API_DOCUMENTATION.md#usage-examples)
- **Common Patterns**: [Quick Reference - Common Use Cases](QUICK_REFERENCE.md#common-use-cases)
- **Complete Workflow**: [Quick Reference - Complete Workflow Example](QUICK_REFERENCE.md#complete-workflow-example)

#### Troubleshooting
- **Common Issues**: [API Documentation - Troubleshooting](API_DOCUMENTATION.md#troubleshooting)
- **Quick Fixes**: [Quick Reference - Troubleshooting](QUICK_REFERENCE.md#troubleshooting)
- **Technical Debugging**: [Component Documentation - Error Handling](COMPONENT_DOCUMENTATION.md#error-handling)

#### Technical Implementation
- **Architecture Overview**: [Component Documentation - Technical Architecture](COMPONENT_DOCUMENTATION.md#technical-architecture)
- **Docker Integration**: [Component Documentation - Docker Integration](COMPONENT_DOCUMENTATION.md#docker-integration)
- **Security Features**: [Component Documentation - Security Implementation](COMPONENT_DOCUMENTATION.md#security-implementation)

### 🎯 Use Case Scenarios

#### Scenario 1: Setting up Robot Framework tests for the first time
1. Read [README.md](README.md) for basic understanding
2. Follow [Quick Reference - Quick Start](QUICK_REFERENCE.md#quick-start)
3. Use [Quick Reference - File Structure Examples](QUICK_REFERENCE.md#file-structure-examples) for project setup

#### Scenario 2: Configuring advanced parallel execution
1. Check [API Documentation - robot_threads parameter](API_DOCUMENTATION.md#robot_threads)
2. Review [Quick Reference - Parallel Execution](QUICK_REFERENCE.md#parallel-execution)
3. Consult [Component Documentation - Performance Considerations](COMPONENT_DOCUMENTATION.md#performance-considerations)

#### Scenario 3: Troubleshooting browser crashes
1. Check [Quick Reference - Browser Crashes](QUICK_REFERENCE.md#browser-crashes)
2. Review [API Documentation - Shared Memory Errors](API_DOCUMENTATION.md#shared-memory-errors)
3. Understand [Component Documentation - Resource Allocation](COMPONENT_DOCUMENTATION.md#resource-allocation)

#### Scenario 4: Understanding the technical implementation
1. Start with [Component Documentation - Technical Architecture](COMPONENT_DOCUMENTATION.md#technical-architecture)
2. Review [Component Documentation - Action Definition Component](COMPONENT_DOCUMENTATION.md#action-definition-component)
3. Study [Component Documentation - Execution Script Component](COMPONENT_DOCUMENTATION.md#execution-script-component)

## Documentation Features

### ✅ Complete Coverage
- **All Public APIs**: Every input parameter documented with types, defaults, and examples
- **All Functions**: Complete function specifications and implementations
- **All Components**: Detailed breakdown of action.yml and test.sh components
- **All Use Cases**: Common patterns, troubleshooting, and best practices

### 📋 Structured Information
- **Consistent Format**: All documentation follows the same structure and format
- **Cross-References**: Links between related sections across documents
- **Progressive Detail**: From basic usage to technical implementation
- **Quick Access**: Tables, examples, and quick reference sections

### 🔧 Practical Examples
- **Real-World Scenarios**: Examples based on actual usage patterns
- **Copy-Paste Ready**: Complete YAML configurations and code snippets
- **Progressive Complexity**: From simple to advanced configurations
- **Troubleshooting Guides**: Step-by-step resolution for common issues

### 🛡️ Best Practices
- **Security Guidelines**: Proper handling of sensitive data and Docker security
- **Performance Optimization**: Memory, parallel execution, and resource management
- **Maintenance Tips**: Version management, updates, and monitoring
- **Integration Patterns**: GitHub Actions workflow integration and artifact handling

## Contributing to Documentation

### Documentation Standards
- **Consistency**: Follow established format and structure
- **Completeness**: Include all parameters, examples, and edge cases
- **Accuracy**: Ensure all examples work and information is current
- **Clarity**: Use clear language and provide context for technical concepts

### Documentation Updates
- **Version Tracking**: Update documentation with each release
- **Change Log**: Document breaking changes and new features
- **Example Updates**: Ensure all examples work with current versions
- **Cross-Reference Updates**: Maintain links between documents

## Support and Resources

### External Resources
- [Robot Framework Documentation](https://robotframework.org/robotframework/)
- [ppodgorsek/robot-framework Docker Image](https://github.com/ppodgorsek/docker-robot-framework)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [pabot Parallel Execution](https://github.com/mkorpela/pabot)

### Community Support
- **Issues**: Report documentation issues or request improvements
- **Discussions**: Ask questions and share usage patterns
- **Contributions**: Submit documentation improvements and examples

---

**Last Updated**: This documentation index is maintained with the latest version of the Robot Framework Docker Action.

**Documentation Version**: v2.0.0