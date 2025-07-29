# Test Case Generation Workflow

This repository contains the workflow and rules for generating test cases and managing them in BrowserStack Test Management.

## Project Structure

The workspace is organized to handle test case generation from multiple sources:
- Jira tickets
- Figma designs
- Confluence documentation
- Product Informations 

## Test Case Generation Rules

### Trigger Conditions
1. When message contains "Create test scenarios for" with a Jira Task ID (e.g., PA-12345)
2. When message contains "Add the test scenarios that you create to BrowserStack"

### Information Sources
- Jira Task Description
- Figma Design Analysis (if URL provided)
- Confluence Documentation (if URL provided)


### User Prompt Examples:
    Create test scenarios for Jira Task SD-121382
    
    Read the description of Jira Task SD-121382
    
    Product Information: @canvasRules.mdc , @canvasTools.mdc
    
    Read @browserstack.mdc
    
    Generate test scenarios with these informations.
    
    Add the test scenarios that you create to Browserstack.
    Project Name: Minerva
    Folder Name: AI Cases 
