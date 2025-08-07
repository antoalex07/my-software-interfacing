# TestService - Universal Communication Service

A Windows service that provides TCP/IP and RS232 serial communication capabilities for hex message exchange with XML processing.

## Features

### Communication Types
- **TCP/IP Communication**
  - Client mode: Connect to remote TCP server
  - Server mode: Listen for incoming TCP connections
  - Auto-reconnection with configurable intervals
  - Keep-alive support

- **RS232 Serial Communication**
  - Full serial port configuration support
  - Hardware and software flow control
  - Configurable message delimiters
  - Port monitoring and auto-reconnection

### Message Processing
- Hex format message handling
- XML content detection and formatting
- Bidirectional communication logging
- Message timestamping and direction tracking

### Service Features
- Windows Service compatibility
- Hot configuration reloading
- Comprehensive logging with Serilog
- Error handling and recovery
- Configuration file monitoring

## Configuration

### Basic Configuration Structure

The service uses a `config.json` file that supports both TCP and Serial communication settings:

```json
{
  "CommunicationType": "TCP|SERIAL",
  "IpAddress": "IP address for TCP",
  "Port": 8080,
  "ConnectionMethod": "client|server",
  "PortName": "COM1",
  "BaudRate": 9600,
  "Parity": "None|Odd|Even|Mark|Space",
  "DataBits": 8,
  "StopBits": "None|One|Two|OnePointFive",
  "Handshake": "None|XOnXOff|RequestToSend|RequestToSendXOnXOff",
  "DtrEnable": false,
  "RtsEnable": false,
  "ReconnectIntervalSeconds": 30,
  "ConnectionTimeoutSeconds": 10,
  "ReceiveTimeoutSeconds": 30,
  "SendTimeoutSeconds": 10,
  "EnableKeepAlive": true,
  "KeepAliveIntervalSeconds": 60,
  "MaxReconnectAttempts": -1,
  "LogLevel": "Information",
  "MessageDelimiter": "",
  "ReadBufferSize": 4096,
  "WriteBufferSize": 2048
}
```

### TCP Configuration Examples

#### TCP Client Mode
```json
{
  "CommunicationType": "TCP",
  "IpAddress": "192.168.1.100",
  "Port": 8080,
  "ConnectionMethod": "client",
  "ReconnectIntervalSeconds": 30,
  "EnableKeepAlive": true,
  "MaxReconnectAttempts": -1
}
```

#### TCP Server Mode
```json
{
  "CommunicationType": "TCP",
  "IpAddress": "0.0.0.0",
  "Port": 8080,
  "ConnectionMethod": "server",
  "ReconnectIntervalSeconds": 30,
  "EnableKeepAlive": true
}
```

### Serial RS232 Configuration Examples

#### Standard Serial Configuration
```json
{
  "CommunicationType": "SERIAL",
  "PortName": "COM1",
  "BaudRate": 9600,
  "Parity": "None",
  "DataBits": 8,
  "StopBits": "One",
  "Handshake": "None",
  "MessageDelimiter": "\r\n"
}
```

#### High-Speed Serial with Hardware Flow Control
```json
{
  "CommunicationType": "SERIAL",
  "PortName": "COM3",
  "BaudRate": 115200,
  "Parity": "None",
  "DataBits": 8,
  "StopBits": "One",
  "Handshake": "RequestToSend",
  "DtrEnable": true,
  "RtsEnable": true
}
```

#### Industrial Serial Configuration
```json
{
  "CommunicationType": "SERIAL",
  "PortName": "COM2",
  "BaudRate": 19200,
  "Parity": "Even",
  "DataBits": 7,
  "StopBits": "Two",
  "Handshake": "XOnXOff",
  "MessageDelimiter": "\n"
}
```

## Configuration Parameters

### Communication Type
- **CommunicationType**: `"TCP"` or `"SERIAL"` - Determines the communication method

### TCP/IP Settings
- **IpAddress**: Target IP address or local bind address (for server mode)
- **Port**: TCP port number
- **ConnectionMethod**: `"client"` or `"server"`

### Serial RS232 Settings
- **PortName**: Serial port name (e.g., "COM1", "COM2")
- **BaudRate**: Communication speed (300, 1200, 2400, 4800, 9600, 19200, 38400, 57600, 115200)
- **Parity**: Error checking (`"None"`, `"Odd"`, `"Even"`, `"Mark"`, `"Space"`)
- **DataBits**: Number of data bits (5, 6, 7, 8)
- **StopBits**: Stop bits (`"None"`, `"One"`, `"Two"`, `"OnePointFive"`)
- **Handshake**: Flow control method
  - `"None"`: No flow control
  - `"XOnXOff"`: Software flow control
  - `"RequestToSend"`: Hardware flow control (RTS/CTS)
  - `"RequestToSendXOnXOff"`: Both hardware and software flow control
- **DtrEnable**: Enable Data Terminal Ready signal
- **RtsEnable**: Enable Request to Send signal
- **MessageDelimiter**: Optional message delimiter (e.g., `"\r\n"`, `"\n"`)

### Common Settings
- **ReconnectIntervalSeconds**: Time between reconnection attempts
- **ConnectionTimeoutSeconds**: Connection timeout (TCP only)
- **ReceiveTimeoutSeconds**: Receive operation timeout
- **SendTimeoutSeconds**: Send operation timeout
- **EnableKeepAlive**: Enable periodic keep-alive messages
- **KeepAliveIntervalSeconds**: Interval between keep-alive messages
- **MaxReconnectAttempts**: Maximum reconnection attempts (-1 for infinite)
- **LogLevel**: Logging level (`"Debug"`, `"Information"`, `"Warning"`, `"Error"`)
- **ReadBufferSize**: Read buffer size in bytes
- **WriteBufferSize**: Write buffer size in bytes

## Installation and Usage

### Prerequisites
- .NET 8.0 or later
- Windows OS (for Windows Service functionality)
- Serial port drivers (for RS232 communication)

### Building the Application
```bash
dotnet build
dotnet publish -c Release
```

### Running as Console Application
```bash
dotnet run
```

### Installing as Windows Service
```bash
sc create "TestService" binpath="C:\path\to\TestService.exe"
sc start "TestService"
```

### Configuration Management
1. Place `config.json` in the application directory
2. Modify settings as needed
3. The service will automatically reload configuration when the file changes
4. Check logs for configuration validation and loading status

## Logging

The service creates logs in the following locations:
- **Application logs**: `C:\Logs\MyService\app-log.txt`
- **Error logs**: `C:\Logs\MyService\error-log.txt`
- **Message logs**: `C:\Temp\message-log.txt`
- **Connection status**: `C:\Temp\connection-status.txt`
- **Console output**: Available when running in console mode

## Message Format

### Hex Message Format
- All messages are processed in hexadecimal format
- Example: `"48656C6C6F20576F726C64"` (represents "Hello World")

### XML Content Detection
- The service automatically detects and formats XML content within hex messages
- XML content is logged separately for readability
- Malformed XML is handled gracefully

### Message Direction
- **Sent**: Messages sent by the service
- **Received**: Messages received from external devices

## Troubleshooting

### Common Issues

#### TCP Connection Issues
- Verify IP address and port configuration
- Check firewall settings
- Ensure target device is accessible
- Review connection timeout settings

#### Serial Port Issues
- Verify COM port availability using Device Manager
- Check if port is not in use by another application
- Validate baud rate and communication parameters
- Ensure proper cable connections

#### Configuration Issues
- Validate JSON syntax using a JSON validator
- Check file permissions for config.json
- Review application logs for configuration errors
- Ensure all required parameters are specified

### Debugging
1. Set `LogLevel` to `"Debug"` in configuration
2. Monitor console output when running in console mode
3. Check log files for detailed error information
4. Use Windows Event Viewer for service-related issues

## Customization

### Message Processing
Modify the `ProcessReceivedMessage` method in `Worker.cs` to implement custom message handling logic.

### Communication Protocols
Extend the `INetworkService` interface to support additional communication protocols.

### Error Handling
Customize error handling and recovery mechanisms in the respective service classes.

## Support

For issues and questions, refer to the application logs and ensure proper configuration according to your specific communication requirements.
