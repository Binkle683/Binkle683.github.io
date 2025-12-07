# Smart Greenhouse IoT Web Interface

## Project Overview

This is a web-based monitoring and control interface for the Smart Greenhouse IoT Environment & Security Monitoring System. The interface provides real-time sensor data visualization and remote control capabilities through MQTT protocol.

## Features

### Real-time Monitoring
- **Environment Monitoring Card**:
  - Temperature display with progress bar (0-50°C range, 0.1°C precision)
  - Humidity display with progress bar (50-100% range, converted from ADC)
  - Light intensity display with progress bar (0-100% brightness, converted from ADC)

- **Security Monitoring Card**:
  - Person detection (PIR sensor)
  - Tilt detection (with 5-second confirmation)
  - Alarm (Buzzer) status and control

### Remote Control
- **RGB LED Control**: Manual control of LED count (0-5) or auto mode
- **Temperature Threshold Control**: Set fan activation threshold (25.0-50.0°C)
- **Buzzer Control**: Manual ON/OFF or auto mode (triggered by tilt detection)

### Data Visualization
- Temperature history chart
- Humidity history chart
- Real-time sensor data updates via MQTT

### User Interface
- Modern, responsive design
- Color-coded progress bars for visual feedback
- Real-time status indicators
- Connection log for debugging

## Deployment

### GitHub Pages Deployment (Recommended)

#### Method 1: Using GitHub Actions (Auto-deploy)

1. Push code to GitHub repository
2. Enable GitHub Pages in repository settings:
   - Go to **Settings** → **Pages**
   - Select **GitHub Actions** as source
3. Web interface will auto-deploy on each push to main/master branch

#### Method 2: Manual Deployment

1. Copy all files from `web` directory to repository root
2. Enable GitHub Pages:
   - Go to **Settings** → **Pages**
   - Select **main branch** or **master branch** as source
   - Select **/ (root)** as folder
3. Access at `https://yourusername.github.io/repository-name/`

#### Method 3: Using gh-pages Branch

1. Create and switch to gh-pages branch:
   ```bash
   git checkout -b gh-pages
   ```

2. Copy files from `web` directory to root

3. Commit and push:
   ```bash
   git add .
   git commit -m "Deploy to GitHub Pages"
   git push origin gh-pages
   ```

4. Enable GitHub Pages:
   - Go to **Settings** → **Pages**
   - Select **gh-pages branch** as source

## Local Testing

Simply open `index.html` in a web browser to test the interface locally.

## Configuration

### MQTT Settings

编辑 `web_config.yaml` 文件来配置 MQTT 连接参数：

Edit `web_config.yaml` file to configure MQTT connection parameters:

```yaml
mqtt:
  # MQTT 服务器地址（WebSocket 协议）
  host: "wss://your-mqtt-server:8084/mqtt"
  
  # 认证信息
  authentication:
    username: "your_username"
    password: "your_password"
  
  # 客户端配置
  client:
    # 客户端ID前缀（会自动添加随机后缀）
    client_id_prefix: "esp32-smart-home-web-"
    # 清理会话
    clean: true
    # 重连间隔（毫秒）
    reconnect_period: 5000
  
  # MQTT 主题配置
  topics:
    # 传感器数据主题
    sensor: "/public/esp32/smart_home"
    # 报警主题
    alert: "/public/esp32/smart_home/alert"
    # 控制主题
    control: "/public/esp32/smart_home/control"
```

**配置说明**：
- `host`: MQTT 服务器的 WebSocket 地址（使用 `wss://` 表示安全连接，`ws://` 表示非安全连接）
- `authentication.username`: MQTT 用户名
- `authentication.password`: MQTT 密码
- `topics.*`: MQTT 主题名称，需要与 ESP32 设备配置保持一致

**注意事项**：
- 如果 `web_config.yaml` 文件无法加载，系统会使用默认配置（硬编码在 HTML 中）
- 确保 YAML 文件格式正确，缩进使用空格（不是制表符）
- 配置文件需要与 `index.html` 放在同一目录下

## Control Commands

### RGB LED Control
- **Set LED Count**: Send `{"rgb_led_count": 3}` to control topic
- **Auto Mode**: Send `{"auto_mode": true}` to restore automatic control

### Temperature Threshold
- **Set Threshold**: Send `{"temp_threshold": 28.5}` to control topic (range: 25.0-50.0°C)

### Buzzer Control
- **ON**: Send `{"buzzer_control": "ON"}`
- **OFF**: Send `{"buzzer_control": "OFF"}`
- **AUTO**: Send `{"buzzer_control": "AUTO"}`

## Data Format

### Sensor Data (Published to sensor topic)
```json
{
  "temperature": 25.5,
  "humidity": 75.3,
  "light": 2047,
  "person_detected": false,
  "tilt": false,
  "fan_status": "ON",
  "fan_mode": "auto",
  "buzzer_status": "OFF",
  "buzzer_mode": "auto",
  "rgb_led_count": 3,
  "temp_threshold": 28.0,
  "timestamp": 1234567890,
  "device": "ESP32_SmartGreenhouse"
}
```

## Browser Compatibility

- Chrome/Edge (recommended)
- Firefox
- Safari
- Mobile browsers (responsive design)

## Notes

1. **MQTT WebSocket**: Ensure your MQTT server supports WebSocket connections (WSS for secure)
2. **Browser Notifications**: The interface will request notification permission for alerts
3. **Real-time Updates**: Data updates every 2 seconds (configurable in ESP32)
4. **Connection Status**: The status bar shows MQTT connection state in real-time
