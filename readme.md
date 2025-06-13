# 📱 DataMatrix Document Scanner

A pure web-based mobile application for scanning DataMatrix barcodes and processing documents through Vantage Cloud. Built with vanilla JavaScript and the ZXing library for reliable barcode detection, this app can run on any mobile device with a modern browser. The application allows you to capture photos and send them to Vantage skills for processing, automatically detecting DataMatrix barcodes in the live video feed and taking photos automatically when a DataMatrix is present, while providing a manual capture button for users to explicitly take photos when no barcode is detected.

## ✨ Features

- **Universal Compatibility**: Pure web-based app runs on any mobile device
- **DataMatrix Barcode Detection**: Automatic scanning using ZXing library with real-time video analysis
- **Automatic Photo Capture**: Takes photos automatically when DataMatrix is detected
- **Manual Capture**: Fallback option when auto-detection fails - user can explicitly capture
- **Vantage Integration**: Direct upload and processing with Vantage Cloud skills
- **Mobile-First Design**: Optimized for mobile devices with responsive UI
- **Real-time Camera Feed**: Live video preview with scanning overlay
- **Connection Testing**: Validate credentials before scanning
- **HTTPS Security**: Required for camera access on modern browsers

## 🚀 Quick Start

1. **Clone or download** the project files
2. **Host the file** on an HTTPS server (required for camera access)
3. **Open** `index.html` in a mobile browser
4. **Configure** your Vantage server credentials
5. **Start scanning** DataMatrix barcodes!

## 📋 Requirements

### Browser Support
- Modern mobile browsers (Chrome, Safari, Firefox)
- Camera access permissions
- HTTPS connection (or localhost for testing)

### Vantage Configuration
- Vantage Cloud server
- Client credentials (ID and Secret)
- Configured Skill ID for document processing

## ⚙️ Configuration

Fill in the following fields in the app:

| Field | Description | Example |
|-------|-------------|---------|
| **Base URL** | Your Vantage server URL | `https://your-server.vantage-cloud.com` |
| **Client ID** | OAuth client identifier | `your-client-id` |
| **Client Secret** | OAuth client secret | `your-client-secret` |
| **Skill ID** | Document processing skill | `your-skill-id` |

## 🔧 Vantage Cloud Setup

Before using the application, you need to configure your Vantage Cloud instance to allow API access.

### Step 1: Create API Client in Vantage

1. **Log into** your Vantage Cloud instance
2. **Navigate to** API Clients section
3. **Create** a new API Client
4. **Note down** the Client ID and Client Secret (you'll need these in the app)

### Step 2: Configure API Client Permissions

In the API Client configuration:
- ✅ **Enable** "Client credentials flow"
- ✅ **Grant** the "Skill User" role to the API Client  
- ✅ **Give permission** to use the required skill(s) that will be invoked from the mobile app

### Step 3: Get Skill ID

1. **Navigate to** Vantage Skill Catalog
2. **Find** your desired skill for document processing
3. **Copy** the Skill ID (you'll enter this in the app configuration)

### Configuration Summary

After completing the Vantage setup, you'll have:

| Value | Where to Find | Where to Use |
|-------|---------------|--------------|
| **Base URL** | Your Vantage Cloud instance URL | App configuration |
| **Client ID** | API Client creation (Step 1) | App configuration |
| **Client Secret** | API Client creation (Step 1) | App configuration |
| **Skill ID** | Skill Catalog (Step 3) | App configuration |

## 🔧 Setup Instructions

### Local Development

```bash
# Simple HTTP server for testing
python -m http.server 8000
# Or with Node.js
npx serve .
```

**Note**: For camera access, use `https://localhost:8000` or deploy to HTTPS.

### Production Deployment

1. **Upload** `index.html` to your web server
2. **Ensure HTTPS** is enabled
3. **Test camera permissions** on target devices
4. **Configure** Vantage server settings (see Vantage Cloud Setup above)

## 📱 Usage

### Basic Workflow

1. **Configure** → Enter Vantage server credentials
2. **Test Connection** → Verify authentication
3. **Start Camera** → Begin scanning session
4. **Scan Barcode** → Position DataMatrix code in viewfinder for automatic detection
5. **Process Document** → Automatic photo capture and upload to Vantage skill

### Manual Capture

If automatic detection fails:
1. Position the document clearly in the camera view
2. Tap **"Take Picture Now"** button to explicitly capture
3. The image will be captured and processed without barcode detection


## 🛠️ Technical Details

### Libraries Used

- **ZXing-js**: DataMatrix and barcode detection
  - License: Apache 2.0
  - CDN: `https://unpkg.com/@zxing/library@latest/umd/index.min.js`

### API Endpoints

The app uses these Vantage Cloud endpoints:

```
POST /auth2/connect/token          # Authentication
POST /api/publicapi/v1/transactions # Create transaction
POST /api/publicapi/v1/transactions/{id}/files # Upload image
POST /api/publicapi/v1/transactions/{id}/start # Start processing
```

### Browser APIs

- **MediaDevices.getUserMedia()**: Camera access
- **Canvas API**: Image capture from video stream
- **Fetch API**: HTTP requests to Vantage server
- **FormData**: File upload handling

## 📄 File Structure

```
MobileCapture/
├── index.html          # Main application file
├── README.md          # This documentation
└── (generated files)
    ├── captured images
    └── transaction logs
```

## 🔒 Security Considerations

- **HTTPS Required**: Camera access needs secure context
- **Credentials Storage**: Client secrets stored in browser memory only
- **CORS Configuration**: Server must allow cross-origin requests
- **Token Management**: Access tokens are temporary and auto-refreshed

## 🎨 Customization

### Styling
All CSS is embedded in `index.html`. Key classes:
- `.app-container`: Main application wrapper
- `.scanner-overlay`: Camera scanning overlay
- `.status-indicator`: Status message styling

### Barcode Detection
Modify `initializeZXingDetection()` to adjust:
- Detection frequency
- Supported barcode formats
- Error handling behavior


---

**Note**: This application requires camera permissions and HTTPS hosting for production use. Test thoroughly on your target devices before deployment.