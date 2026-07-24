# Release Notes

## Version 1.3.0

### New Features

#### Monitoring & Execution Tracking
- **Block Status Monitoring Service Integration**: New monitoring service integration for real-time block status updates
  - Visualize individual block execution states and communication points
- **Enhanced Execution Parameters Visualization**: 
  - Full visibility of workflow start and run parameters in monitoring view
  - Detailed execution settings display with better layout and readability

#### Standard Templates and Workflows
- **PROOF-Importer Integration**: Replaced data.sql with proof-importer for database initialization
  - More robust and maintainable approach for initial data population
- **Standard Templates**: Added more standard templates and updated default values

#### User Interface Improvements
- **Template Usage Visualization**: View all workflows and blocks that depend on a specific template
  - Prevents accidental deletion of in-use templates
- **Alphabetical Sorting**: Consistent sorting across all tables and lists
  - Inputs displayed alphabetically in template editor and block configuration
  - Executions sorted by startedAt timestamp

#### Backend Improvements
- **Loop Handling Improvements**: Added `startValue` field for block inputs to support iterative/loop scenarios
  - Enables proper initialization of inputs that participate in feedback loops
  - Distinguishes between static initial parameters and runtime execution values
- **Java Wrapper Implementation**: New Java wrapper for Java-based model program execution
  - Fixed command parsing to avoid jar startup issues
- **Input/Output Type Improvements**:
  - New array data types: STRING_ARRAY, INTEGER_ARRAY, FLOAT_ARRAY, OBJECT_ARRAY
  - FILE_NAME type support for file parameter handling
- **Synchronization Strategy Extension**: Add new strategy *ALL_VALUES*
  - *ALL_VALUES* triggers model execution when all required values are available

### Bug Fixes & Improvements

#### Stability & Reliability
- **Block Container Memory Management**: Fixed block container map clearing before re-population
  - Prevents memory leaks from accumulated block references
  - Ensures proper cleanup between workflow executions
- **Synchronized Input Access**: Thread-safe access to currentInputs map in worker
  - Prevents race conditions in concurrent input handling
  - Improves stability under load
- **Error Step Handling**: Corrected ERROR_STEP emission logic
  - Only sends ERROR_STEP on actual timeout when waiting for inputs
  - Reduces false positive error states

#### Configuration & Consistency
- **Export Functionality**: Fixed missing files folder in execution export
  - Complete ZIP exports now include all necessary file references
  - Improved export reliability for sharing executions

---

## Version 1.2.0

### New Features

#### Workflow Configuration
- **MQTT Integration**: 
  - New MQTT Subscriber and Publisher attachments for IoT and real-time data streaming integration
- **EBM (Energy Building Model) Workflow Support**: 
  - New workflow templates for energy building model simulations and research workflows
- **Workflow Naming Convention**: Demo workflows now clearly marked with `demo_` prefix

#### Block Customization & Configuration
- **Block Parameter Overrides**: Blocks can now override template defaults with fine-grained control
  - Customize container image per block
  - Configure sync strategy at block level
  - Mark shutdown-relevant parameters
  - Track which values differ from template defaults
- **Improved Block Visualization**: Enhanced visual representation of block properties in the editor
  - Better parameter visibility

#### User Interface & Monitoring Enhancements
- **Comprehensive Monitoring View**:
  - Full execution parameters and settings visualization
  - Enhanced ExecutionSettingsPanel with workflow start and run parameters
  - Better visibility into execution configuration and state
  - Improved parameter display for debugging and analysis
- **Consistent Data Organization**:
  - Alphabetical sorting of inputs in template editor and configuration
  - Alphabetical sorting of blocks in configuration views
  - Alphabetical sorting of templates in usage lists
  - Improved discoverability and consistency across UI
- **Template Management Dialog**: Simplified and enhanced templates display with better UX
- **Enhanced Localization**: Complete multi-language support (English/German) for all new features
- **Template Usage Tracking**: 
  - View all workflows and blocks that depend on a specific template
  - Prevents accidental deletion of in-use templates with dependency visualization
  - Enhanced template management with usage information in the UI

### Bug Fixes & Improvements

- Adjusted block logging queue names for consistency
- Added robust error handling for block status updates in orchestrator
  - Prevents orchestrator crashes from individual block errors
- Fixed block container map that was preventing memory cleanup across multiple workflow runs

### Performance & Infrastructure

- **Improved Bundle Size Management**: UI redesigned with better code organization
- **Enhanced State Management**: Better provider and state handling for new features
- **MQTT Library Support**: paho-mqtt integration for IoT communication patterns

---

## Version 1.1.0

### New Features

#### Execution Management
- **Execution Export**: Export complete workflow executions as ZIP files, including all configurations and attachments
  - Non-admin users can export their executions
- **Execution Progress Tracking**: Real-time visualization of execution progress with communication point (CP) tracking
  - Current communication point is stored in the database during execution
  - Progress bars and visualizations in the monitoring interface
- **Execution Logging**: Enhanced logging capabilities for debugging and monitoring workflow executions

#### File Management
- **User Data Directory**: New dedicated `userdata` directory for user-managed files accessible by workflow blocks (e.g., FileReader)
- **Attachments Directory**: Separate `attachments` directory for workflow-specific attachments with subdirectory support
- **File Upload/Download API**: New file controller endpoints for uploading and downloading files via the API
- **Enhanced Attachment Handling**: Improved attachment paths and ZIP entry handling during execution export

#### Workflow Configuration
- **Configurable Shutdown Timeout**: Users can now configure the shutdown timeout for workflows via configuration settings
- **PROOF_DIR Environment Variable**: Support for custom PROOF directory paths via environment variables
- **Input/Output Visualization**: Detailed visualization of input values and workflow parameters in the monitoring view
- **Text Color Customization**: Added `text_Color` field for block customization in the UI

#### User Interface Improvements
- **Admin Role Management**: New utility function to check user roles and permissions
- **Improved Pagination**: Default pagination size adjusted to 25 items for better usability
- **Enhanced Monitoring Interface**: 
  - Better visualization of block status and execution progress
  - Improved layout and readability in monitoring details
  - Enhanced edge and node context menus in the editor

#### Development & Infrastructure
- **MQTT Support**: Added paho-mqtt library to worker for MQTT-based communication capabilities
- **Automatic Image Download**: PROOF now automatically downloads missing Docker images when needed, eliminating the need for manual image downloads
- **Logging Queue Updates**: Updated logging queue names for improved log management
- **Fixed Orchestrator Logging Issues**: Resolved various logging-related bugs and inconsistencies
- **Enhanced Error Handling**: Improved exception handling and error messages across components
- **Updated Python Runtime Environment for Worker images**: Use venv now

---

## Version 1.0.0

### Initial Release
- First public release of PROOF
- Docker-based workflow execution
- Python support for model programs
- Web-based UI for workflow creation and monitoring
- File-based data exchange
- Core blocks: FileReader, FileWriter, and data type converters
