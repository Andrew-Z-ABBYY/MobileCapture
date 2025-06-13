# 📱 DataMatrix Document Scanner

A pure web-based mobile application for scanning DataMatrix barcodes and processing documents through Vantage Cloud. Built with vanilla JavaScript and the ZXing library for reliable barcode detection, this app can run on any mobile device with a modern browser. The application allows you to capture photos and send them to Vantage skills for processing, automatically detecting DataMatrix barcodes in the live video feed and taking photos automatically when a DataMatrix is present, while providing a manual capture button for users to explicitly take photos when no barcode is detected.

## ✨ Features

- **Universal Compatibility**: Pure web-based app runs on any mobile device
- **DataMatrix Barcode Detection**: Automatic scanning using ZXing library with real-time video analysis
- **Automatic Photo Capture**: Takes photos automatically when DataMatrix is detected
- **Manual Capture**: Fallback option when auto-detection fails - user can explicitly capture
- **Vantage Integration**: Direct upload and processing with Vantage Cloud skills using Resource Owner Password Credentials flow
- **Mobile-First Design**: Optimized for mobile devices with responsive UI
- **Real-time Camera Feed**: Live video preview with scanning overlay
- **Connection Testing**: Validate credentials before scanning
- **HTTPS Security**: Required for camera access on modern browsers

## 🧪 Testing the Application

### Live Demo

You can test the application immediately using hosted demo:

**🌐 [https://andrew-z-abbyy.github.io/MobileCapture/](https://andrew-z-abbyy.github.io/MobileCapture/)**

### Testing Steps

1. **Configure Vantage** following the guidelines provided in the Vantage Cloud Setup section
2. **Print test document**: Use the sample PDF with DataMatrix barcode located in the `test/` folder of this repository
3. **Open the demo** on your mobile device or laptop
4. **Enter your Vantage credentials** in the app configuration
5. **Test connection** to verify your setup
6. **Start camera** and point it at the printed DataMatrix barcode
7. **Watch automatic detection** - the app should detect the barcode and capture the photo automatically
8. **Check Vantage transactions** in your Vantage Cloud instance to see the processed document

### Expected Results

- ✅ DataMatrix barcode should be detected automatically
- ✅ Photo should be captured when barcode is found
- ✅ Document should appear in your Vantage transactions
- ✅ Processing should start automatically in Vantage

## 🔧 Vantage Cloud Setup

Before using the application, you need to configure your Vantage Cloud instance to allow API access using the Resource Owner Password Credentials flow.

### Step 1: Get Tenant ID

1. **Navigate to** Configuration → General
2. **Copy** your Tenant ID (GUID format)
3. **Note**: This is different from tenant name - it's a UUID like `12345678-1234-1234-1234-123456789abc`

### Step 2: Create API Client in Vantage

1. **Log into** your Vantage Cloud instance
2. **Navigate to** Configuration → Public API Client section
3. **Create** a new API Client
4. **Enable** "Resource Owner Password Credentials" flow
5. **Note down** the Client ID and Client Secret (you'll need these in the app)

### Step 3: Get Skill ID

1. **Navigate to** Vantage Skill Catalog
2. **Find** your desired skill for document processing and click "Preview skill"
3. **Copy** the Skill ID from the "More info" section of skill preview panel (you'll enter this in the app configuration)

### Authentication Flow Details

The application uses the **Resource Owner Password Credentials** OAuth 2.0 flow:

- **Authentication URL**: `https://your-vantage-url/auth2/{tenantId}/connect/token`
- **Grant Type**: `password`
- **Scope**: `openid permissions global.wildcard`
- **Required Parameters**: username, password, client_id, client_secret

**Important Notes**:
- If your email is associated with multiple tenants, the tenant-specific authentication endpoint is used
- If your account uses external authentication (Google, Microsoft, etc.), this flow may not be available
- Both Vantage URL and Tenant ID are required for proper authentication

### Configuration Summary

After completing the Vantage setup, you'll have:

| Value | Where to Find | Where to Use |
|-------|---------------|--------------|
| **Vantage URL** | Your Vantage Cloud instance URL | App configuration |
| **Tenant ID** | Settings → Tenant Information | App configuration |
| **Client ID** | API Client creation (Step 1) | App configuration |
| **Client Secret** | API Client creation (Step 1) | App configuration |
| **Username** | Your login email | App configuration |
| **Password** | Your login password | App configuration |
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
POST /auth2/{tenantId}/connect/token        # Authentication (tenant-specific)
POST /api/publicapi/v1/transactions         # Create transaction
POST /api/publicapi/v1/transactions/{id}/files # Upload image
POST /api/publicapi/v1/transactions/{id}/start # Start processing
```

### Browser APIs

- **MediaDevices.getUserMedia()**: Camera access
- **Canvas API**: Image capture from video stream
- **Fetch API**: HTTP requests to Vantage server
- **FormData**: File upload handling

## 🔒 Security Considerations

- **HTTPS Required**: Camera access needs secure context
- **Credentials Storage**: Client secrets stored in browser memory only
- **CORS Configuration**: Server must allow cross-origin requests
- **Token Management**: Access tokens are temporary and auto-refreshed
- **Authentication Method**: Uses Resource Owner Password Credentials flow - not suitable if external IdP is configured

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