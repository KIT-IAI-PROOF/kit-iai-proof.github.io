# Release Notes

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
