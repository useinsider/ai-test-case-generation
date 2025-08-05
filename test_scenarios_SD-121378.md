# Test Scenarios for Jira Task SD-121378 - Call an API Element

## Test Suite Overview
This document contains comprehensive test scenarios for the Call an API element functionality in Insider's Architect platform, covering all major features including request configuration, response management, throttling, retry mechanisms, and integration capabilities.

---

## 1. Basic Functionality Tests

### TC-001: Basic API Call Configuration
**Objective:** Verify basic Call an API element can be created and configured
**Preconditions:** User has access to Architect platform
**Test Steps:**
1. Navigate to Architect
2. Click "Start From Scratch"
3. Add any Starter element
4. Click the `+` icon
5. Select "Channels > Call an API"
6. Click on the added Call an API element
7. Configure basic request settings:
   - Enter endpoint URL
   - Select HTTP method (GET)
   - Add basic headers
8. Click "Save"

**Expected Results:**
- Call an API element is successfully added to canvas
- Element appears filled when configuration is valid
- Element can be saved without errors
- Hover shows summary of configuration

### TC-002: HTTP Method Support
**Objective:** Verify all supported HTTP methods work correctly
**Test Steps:**
1. Create Call an API element
2. Test each HTTP method:
   - GET
   - POST
   - PUT
   - DELETE
3. Configure appropriate headers and body for each method
4. Click "Test API" for each configuration

**Expected Results:**
- All four HTTP methods are available in dropdown
- Each method sends appropriate request format
- Test API returns expected status codes

### TC-003: Dynamic Content in Request Body
**Objective:** Verify dynamic content insertion in JSON payload
**Test Steps:**
1. Configure Call an API element with POST method
2. Add dynamic content to request body:
   - User attributes: `{d_uuid}`, `{em}`
   - Event parameters: `{cart_page_view.na}`
   - Custom attributes
3. Use "Add Dynamic Content" feature
4. Test with both placeholder and real data

**Expected Results:**
- Dynamic content is properly inserted into JSON payload
- Special characters are auto-encoded
- JSON structure remains valid
- Fallback values work when dynamic content is null

---

## 2. Response Management Tests

### TC-004: Enable Response Storage
**Objective:** Verify response storage functionality
**Test Steps:**
1. Configure Call an API element
2. Toggle "Enable Call an API Response" under payload area
3. Add response variables:
   - Variable Name: "user_status"
   - Response Snippet: "$.status"
   - Data Type: "string"
4. Add multiple variables (up to 150)
5. Save configuration

**Expected Results:**
- Response storage toggle works correctly
- Variables can be added and configured
- Data type selection works (string, number, date)
- Configuration saves without errors

### TC-005: Response Variable Usage
**Objective:** Verify response variables can be used in downstream elements
**Test Steps:**
1. Configure Call an API with response variables
2. Add Update User Attribute element after Call an API
3. Map response variables to UCD attributes
4. Add Check API Response element
5. Use response variables in conditions

**Expected Results:**
- Response variables are available in downstream elements
- Variables can be mapped to user attributes
- Check API Response can use response variables in conditions

### TC-006: Response Data Types
**Objective:** Verify different data types are handled correctly
**Test Steps:**
1. Configure response variables with different data types:
   - String: `"user_name"`
   - Number: `"user_score"`
   - Date: `"last_login"`
2. Test with API responses containing each data type
3. Verify data type validation

**Expected Results:**
- Each data type is properly parsed and stored
- Invalid data types are handled gracefully
- Date formats are correctly processed

---

## 3. Throttling and Retry Tests

### TC-007: API Preferences Configuration
**Objective:** Verify throttling configuration in API Preferences
**Test Steps:**
1. Navigate to Architect > API Preferences
2. Click "Add API Endpoint"
3. Enter URL and Request Size Limit (between 5-5000, multiples of 5)
4. Save configuration
5. Apply throttled URL in Call an API element

**Expected Results:**
- API endpoint can be added successfully
- Request size limits are enforced (multiples of 5)
- Throttled URL appears automatically in Call an API element
- Bypass Throttling toggle works

### TC-008: Throttling Behavior
**Objective:** Verify throttling limits are enforced correctly
**Test Steps:**
1. Configure throttling for endpoint (e.g., 10 req/sec)
2. Send multiple requests rapidly to exceed limit
3. Monitor throttling behavior
4. Check analytics for "Throttled" metrics

**Expected Results:**
- Requests are throttled when limit is exceeded
- Throttled requests are retried up to 10 times
- Retry intervals are random (3-7 minutes)
- "Proceeded (Throttled)" increments in analytics

### TC-009: Timeout Retry Mechanism
**Objective:** Verify timeout retry for 503/504 errors
**Test Steps:**
1. Configure Call an API to endpoint that returns 503/504
2. Send request and monitor retry behavior
3. Check retry attempts in analytics

**Expected Results:**
- 503/504 errors trigger retry mechanism
- Up to 3 retry attempts are made
- Retry intervals are 3-7 minutes
- User proceeds to next element regardless of outcome

---

## 4. Attribution Channel Tests

### TC-010: Attribution Channel Configuration
**Objective:** Verify attribution channel can be enabled and configured
**Test Steps:**
1. Open Call an API element settings
2. Toggle "Attribution Channel"
3. Select channel type (Email, SMS, WhatsApp, App Push, Web Push, Other)
4. Save configuration
5. Launch journey and check analytics

**Expected Results:**
- Attribution Channel toggle works correctly
- All channel types are available for selection
- Configuration saves without validation errors
- Analytics show channel-specific metrics

### TC-011: Attribution Channel Analytics
**Objective:** Verify attribution channel metrics in analytics
**Test Steps:**
1. Configure Call an API with Attribution Channel enabled
2. Launch journey
3. Open Analytics for the element
4. Check for channel-specific metrics and conversions

**Expected Results:**
- Channel-specific metrics are displayed
- Conversions and Revenue show Last-Click attribution
- Metrics are properly categorized by selected channel

---

## 5. Integration Tests

### TC-012: Integration with Update User Attribute
**Objective:** Verify Call an API can integrate with Update User Attribute
**Test Steps:**
1. Configure Call an API with response variables
2. Add Update User Attribute element after Call an API
3. Map response variables to UCD attributes
4. Test data flow through both elements

**Expected Results:**
- Response variables are available in Update User Attribute
- Data is correctly mapped and stored
- User attributes are updated with API response data

### TC-013: Integration with Check API Response
**Objective:** Verify Call an API integrates with Check API Response
**Test Steps:**
1. Configure Call an API with response variables
2. Add Check API Response element after Call an API
3. Use response variables in conditions (eq, ne, gt, lt, contains)
4. Test different condition operators

**Expected Results:**
- Response variables can be used in Check API Response conditions
- All operators work correctly with response data
- User branching works based on API response conditions

### TC-014: Integration with Next Best Channel
**Objective:** Verify Call an API integration with Next Best Channel
**Test Steps:**
1. Enable "Call an API for Next Best Channel" in Gachapon
2. Add Call an API element under Next Best Channel element
3. Check "Attribution Channel" and select channel type
4. Configure API request
5. Launch journey

**Expected Results:**
- Call an API element behaves like selected channel
- Requests are routed to appropriate service
- Attribution Channel validation works correctly
- No validation errors at launch

---

## 6. Error Handling Tests

### TC-015: Invalid Endpoint URL
**Objective:** Verify handling of invalid endpoint URLs
**Test Steps:**
1. Configure Call an API with invalid URL
2. Click "Test API"
3. Check error handling and user feedback

**Expected Results:**
- Clear error message is displayed
- User is informed about invalid URL
- Element can still be saved for later correction

### TC-016: Large Request Body
**Objective:** Verify handling of requests exceeding 100KB limit
**Test Steps:**
1. Configure Call an API with large JSON payload (>100KB)
2. Try to save configuration
3. Check error handling

**Expected Results:**
- Clear error message about size limit
- Configuration cannot be saved until payload is reduced
- User is guided on how to reduce payload size

### TC-017: Invalid JSON Format
**Objective:** Verify JSON validation for request body
**Test Steps:**
1. Configure Call an API with invalid JSON in body
2. Set Content-Type to application/json
3. Try to save configuration

**Expected Results:**
- JSON validation error is displayed
- Configuration cannot be saved until JSON is valid
- Clear error message guides user to fix JSON

---

## 7. Analytics and Monitoring Tests

### TC-018: Analytics Metrics Verification
**Objective:** Verify all analytics metrics are tracked correctly
**Test Steps:**
1. Launch journey with Call an API element
2. Generate various scenarios (success, failure, throttling)
3. Check Analytics for the element
4. Verify all metrics are present and accurate

**Expected Results:**
- All metrics are displayed: Arrived, Processed, Success, Failed, Throttled, Retried, Dropped
- Metrics are accurate and updated in real-time
- GDPR opt-outs are properly tracked as "Dropped"

### TC-019: CloudWatch and SQS Logging
**Objective:** Verify logging to CloudWatch and SQS
**Test Steps:**
1. Configure Call an API with logging enabled
2. Send requests and monitor logs
3. Check CloudWatch logs for processing information
4. Verify SQS message processing

**Expected Results:**
- Requests are logged to CloudWatch
- SQS messages are processed correctly
- Logs contain relevant debugging information
- Success/failure status is logged to Kinesis

---

## 8. Performance Tests

### TC-020: High Volume Request Handling
**Objective:** Verify system handles high volume of requests
**Test Steps:**
1. Configure throttling for high limit (e.g., 5000 req/sec)
2. Send high volume of requests
3. Monitor system performance and response times
4. Check for any degradation or errors

**Expected Results:**
- System handles high volume without errors
- Response times remain acceptable
- No memory leaks or performance degradation
- All requests are processed correctly

### TC-021: Concurrent User Testing
**Objective:** Verify system handles multiple concurrent users
**Test Steps:**
1. Simulate multiple users reaching Call an API element simultaneously
2. Monitor request processing and response times
3. Check for race conditions or data corruption

**Expected Results:**
- All concurrent requests are processed correctly
- No data corruption or race conditions
- Response times remain consistent
- User data is properly isolated

---

## 9. Security Tests

### TC-022: Authorization Header Handling
**Objective:** Verify secure handling of authorization headers
**Test Steps:**
1. Configure Call an API with Authorization header
2. Use different authorization methods (Bearer token, Basic auth)
3. Verify headers are sent securely
4. Test with invalid credentials

**Expected Results:**
- Authorization headers are sent correctly
- Invalid credentials are handled gracefully
- No sensitive data is exposed in logs
- Security best practices are followed

### TC-023: GDPR Compliance
**Objective:** Verify GDPR compliance for data handling
**Test Steps:**
1. Configure Call an API with user data
2. Test with users who have opted out
3. Verify data handling complies with GDPR
4. Check data retention policies

**Expected Results:**
- GDPR opt-outs are respected
- User data is handled according to privacy policies
- No unauthorized data transmission
- Proper data retention and deletion

---

## 10. Template and Reusability Tests

### TC-024: Save and Load Templates
**Objective:** Verify template functionality works correctly
**Test Steps:**
1. Configure Call an API element with complex settings
2. Click "Save Template"
3. Create new journey and add Call an API
4. Select saved template from dropdown
5. Verify all settings are loaded correctly

**Expected Results:**
- Templates can be saved successfully
- Saved templates appear in dropdown
- All configuration settings are preserved
- Templates can be reused across journeys

---

## Test Execution Guidelines

### Prerequisites
- Access to Insider's Architect platform
- Test environment with API endpoints
- Monitoring tools (CloudWatch, Analytics)
- Test data with various user attributes

### Test Data Requirements
- Valid and invalid API endpoints
- Different HTTP methods and status codes
- Various JSON payload sizes and formats
- User data with different attribute combinations
- GDPR-compliant and non-compliant user data

### Success Criteria
- All test scenarios pass without critical errors
- Performance meets specified requirements
- Security and compliance requirements are met
- User experience is smooth and intuitive
- Analytics provide accurate and useful metrics

### Risk Mitigation
- Test in isolated environment before production
- Monitor system resources during high-volume tests
- Have rollback plan for configuration changes
- Document any issues or unexpected behaviors
- Validate with stakeholders before final approval 