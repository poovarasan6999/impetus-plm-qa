Testing Framework – Overview
Centric PLM supports both UI automation (Playwright-based browser testing) and API automation (HTTP-based validation).
 It provides dynamic environment management, centralized data control, and automatic reporting — all in one unified structure.

Framework Architecture
CentricPLM/
├── ui/                         # UI Testing (Browser Automation)
│   ├── features/               # BDD Feature Files
│   ├── step_definitions/       # Step Definitions
│   ├── pom/                    # Page Object Model (Reusable UI Actions)
│   ├── test_data/              # UI Test Data & Credentials
│   └── containers/             # Shared Runtime Storage
│
├── api/                        # API Testing (HTTP Automation)
│   ├── features/               # BDD Feature Files
│   ├── step_definitions/       # Step Implementations
│   ├── payloads/               # Request/Response Payload Classes
│   ├── framework/              # Base API Client
│   ├── test_data/              # Endpoint & Data Configurations
│   └── containers/             # Shared API State Storage
│
├── config/                     # Environment & Credential Management
│   └── global_config.py
│
├── reports/                    # Reports Directory (UI + API)
│   ├── index.html              # Unified Report Index
│   ├── ui/                     # UI Reports
│   └── api/                    # API Reports
│
├── run_tests.py                # Combined Runner (UI + API)
├── generate_api_reports.py     # API Runner (Auto HTML Reports)
├── generate_reports.py         # UI Runner (Auto HTML Reports)
└── requirements.txt            # Python Dependencies


Key Modules Explained
Configuration
global_config.py — central environment manager (DEV, SIT, UAT, PROD)

Defines:
Base URLs
Credentials
Test modes (UI / API / BOTH)

API Layer
Handles API testing with cookie-based authentication (SecurityTokenURL)
Includes payload builders (login_payload.py, style_payload.py)
Supports POST, GET, PATCH, and DELETE requests with built-in logging
Manages dynamic test data through variable_container.py

UI Layer
Built using Playwright for browser automation
Follows the Page Object Model (POM) pattern:
Easy locator management
Reusable page interactions

BDD test flow:
.feature files → readable test scenarios
step_definitions/ → implementation logic
Data isolation using ui_container.py

Reports
Automatically generated HTML, Console, and JSON reports
Auto-opens after test execution
Maintains both UI and API reports separately with a unified index



Execution Commands

Type  Command
      
Output
Run UI Tests
python3 generate_reports.py
UI automation + HTML report
Run API Tests
python3 generate_api_reports.py
API flow + HTML & console reports
Run Both
python3 run_tests.py
Full regression suite (UI + API)


Core Capabilities
Category
Highlights
Environment Control
Switch between UAT, PROD, etc. from config
Test Data Management
Centralized JSON & Python files
UI Automation
Playwright with POM for modular actions
API Automation
RESTful client with reusable payloads
Data Sharing
Containers for step-level and module-level reuse
Error Handling
Built-in logging, retries, and validations
Reporting
Rich HTML, JSON, and console summaries
Scalability
Plug-and-play structure for new tests & modules


API Style Creation Flow (Located in api/)
This part of the framework is handled inside the api/ folder, which automates backend validation through API calls.
Flow Steps (and where they live):
Step
What It Does
Framework Location
Login to PLM API
Authenticates using credentials and generates a secure token
api/payloads/login_payload.py + api/framework/base_api.py
Fetch existing reference IDs
Retrieves valid IDs like category, season, and product type for style creation
api/step_definitions/api_steps.py
Create new Style (POST /styles)
Submits the style creation payload using Base API client
api/payloads/style_payload.py + api/framework/base_api.py
Validate response (GET /styles/{id})
Confirms that the new style exists in the database
api/step_definitions/api_steps.py
Store data in container
Saves style ID, code, and other response data for later UI use
api/containers/variable_container.py


UI Verification Flow (Located in ui/)
This part of the framework, found under the ui/ folder, automates frontend validation using Playwright.
 It verifies that the style created via API is visible and correct in the PLM web application.
Flow Steps (and where they live):
Step
What It Does
Framework Location
Launch browser session
Opens the Centric PLM web application
ui/step_definitions/plm_ui_steps.py
Login to PLM
Logs in using credentials from test data
ui/test_data/login_credentials.json
Search for created style (by code)
Finds the API-created style in the UI grid
ui/pom/plm_style_creation_page.py
Validate lifecycle and brand details
Confirms style properties match API data
ui/pom/plm_style_creation_page.py + ui/step_definitions/plm_ui_steps.py
Capture screenshot & store in report
Takes screenshot and attaches to report
ui/tests/validate_and_store_style_details.py + reports/ui/


Connection Between API and UI Layers
Component
Purpose
Connection
variable_container.py
Stores data like style ID and code
Shared between API & UI
reports/
Stores combined HTML reports for UI and API
Accessible to all runs
config/global_config.py
Handles environment, credentials, and mode
Controls both UI & API
run_tests.py
Runs UI + API together
Unified execution entry point


 CORE TESTING FRAMEWORKS
Playwright – Browser automation for UI testing


Pytest – Core Python testing framework


Pytest-BDD – Behavior-Driven Development (Gherkin support)


Pytest-Asyncio – Asynchronous test execution


Pytest-HTML – HTML report generation


Pytest-JSON-Report – Machine-readable test output


Allure-Pytest – Advanced test analytics and professional reporting



API TESTING
HTTPX – Asynchronous HTTP client (primary)


Requests – Synchronous HTTP client (fallback)


JSON – Serialization and deserialization


Cookie Management – Session persistence and authentication handling


UI TESTING
Playwright – Cross-browser automation


Page Object Model (POM) – Reusable UI design pattern


Dojo/Dijit Framework Handling – Centric PLM-specific UI elements


Locator Strategies – XPath, CSS, and data attribute locators


JavaScript Execution – Direct DOM interactions and custom scripts



REPORTING & VISUALIZATION
HTML Reports – Technical and readable test results


JSON Reports – Data-driven report storage


Allure Reports – Executive-level analytics and dashboards


Console Output Capture – Detailed runtime logs


Screenshots & Visual Evidence – Automatic captures for validation


Custom Styling – CSS/JS enhancements for report presentation



CONFIGURATION & DATA MANAGEMENT
JSON Configuration Files – Environment-specific settings


YAML Support – Optional config format


Environment Variables – Dynamic runtime configuration


Centralized Test Data – Shared data for UI & API tests


Secure Credential Storage – Encrypted login credentials



FRAMEWORK ARCHITECTURE
Page Object Model (POM) – Organized UI automation structure


Step Definitions – BDD implementation layer


Feature Files – Human-readable Gherkin test cases


Data Containers – Shared test data across modules


Base Classes – Common reusable utilities


Utility Functions – Helpers for logging, parsing, and validation



 PYTHON ECOSYSTEM
Python 3.9+ – Core language


Asyncio – Async operations and concurrency


Pathlib – File path management


Datetime – Time and scheduling utilities


Subprocess – Process handling for parallel runs


Webbrowser – Auto-open reports after test completion



UI FRAMEWORK INTEGRATION
Dojo/Dijit Components – Handling dynamic Centric PLM widgets


Dynamic Locators – For unique widget IDs (widget_uniqName_*)


Stable Data Locators – Using data-csi-form-field attributes


Dropdown & Dialog Management – Complex UI interactions


Grid & Table Operations – Automated verification of records



AUTHENTICATION & SECURITY
Cookie-Based Authentication – Session management for PLM


Token Management – API authentication lifecycle


Credential Encryption – Secured credential handling


Session Persistence – Continuous login state across tests


CSRF Protection – Handling Centric PLM security tokens



MONITORING & DEBUGGING
Detailed Console Logging – Step-by-step output


Error Handling – Graceful failure and retry logic


Retry Mechanisms – For flaky or delayed test cases


Screenshot Capture – For visual debugging


Response Time Metrics – API and UI performance tracking


Status Tracking – Continuous test progress updates


Pytest Integration – Seamless test discovery


HTML/Allure Reports – Auto-publish reports post-run


Cross-Environment Deployment – Run on any test stage (DEV → PROD)



PROJECT STRUCTURE OVERVIEW
CentricPLM/
├── api/               # API testing components
├── ui/                # UI testing components
├── config/            # Environment configuration
├── reports/           # All reports (HTML, JSON, Allure)
├── test_data/         # Test inputs and credentials
├── features/          # BDD feature files
├── step_definitions/  # Implementation of steps
└── utils/             # Utility and helper modules


TEST EXECUTION MODES
UI Mode – Runs Playwright UI automation only


API Mode – Executes HTTP-based API tests only


Combined Mode – Runs both API & UI tests together


Regression Mode – Executes the entire suite for validation



DATA HANDLING
VariableContainer – Stores API runtime data


UIContainer – Maintains UI session data


StyleData Container – Holds style-specific details


Test Data Classes – Structured data management




