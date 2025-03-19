# Tidbyt-Shotgrid Integration

![Preview](https://i.redd.it/33ncocix7k1c1.png) 37 CNVs

## Overview

This application connects a Tidbyt display device to the Shotgrid API, allowing you to view real-time data from your Shotgrid instance directly on your Tidbyt device. The app specifically tracks and displays the count of Versions with "cnv" status in active projects.

## Features

- Real-time monitoring of status items in Shotgrid
- Automatic updates by set interval (default 30s)
- Classic Tidbyt visualization with Shotgrid icon
- Log management to prevent excessive logs on logs on logs

## Requirements

- Tidbyt device
- Shotgrid account with API access
- Python 3.11+
- pixlet CLI tool
- shotgun_api3 Python module

## Setup Instructions

### 1. Installation

1. Clone this repository:
   ```
   git clone https://github.com/BakedStudios/baked-tidbyt-app.git
   cd baked-tidbyt-app
   ```

2. Ensure Python 3.11 is installed at:
   ```
   /Library/Frameworks/Python.framework/Versions/3.11/bin/python3
   ```

3. Install required Python modules:
   ```
   pip3 install shotgun_api3
   ```

4. Install pixlet CLI tool:
   ```
   brew install tidbyt/tidbyt/pixlet
   ```
   > ![NOTE]
   > You will need to authenticate pixlet with your Tidbyt account.

5. Create the Tidbyt application directory:
   ```
   mkdir -p /Applications/Tidbyt
   ```

6. Copy all files to the Tidbyt application directory:
   ```
   cp SG_Data.py generated_script.star com.Tidbyt.SG_Data.plist /Applications/Tidbyt/
   ```

### 2. Configuration

1. Edit the `com.Tidbyt.SG_Data.plist` file to configure your environment variables:
   - `TIDBYT_SG_API_KEY`: Your Shotgrid API key
   - `SHOTGUN_URL`: Your Shotgrid instance URL
   - `TIDBYT_DEVICE_NAME`: Your Tidbyt device ID

2. Start the launchd service:
   ```
   launchctl bootstrap gui/$(id -u) /Applications/Tidbyt/com.Tidbyt.SG_Data.plist
   ```

3. Stop the launchd service:
   ```
   launchctl bootout gui/$(id -u) /Applications/Tidbyt/com.Tidbyt.SG_Data.plist
   ```

## How It Works

1. The Python script `SG_Data.py` connects to Shotgrid via the API
2. It queries for all Versions with "cnv" status in active projects
3. The count is embedded into a Starlark template file
4. pixlet renders the Starlark code to a WebP image
5. The image is pushed to your Tidbyt device
6. The launchd service repeats this process every 30 seconds

## Customization

### Modifying the Query

To track different items, edit the `filters` variable in `SG_Data.py`:

```python
# Example: Find all tasks with status "in_progress"
filters = [['sg_status_list', 'is', "ip"], ['project.Project.sg_status', 'is', 'Active']]
result = sg.find('Task', filters)
```

### Changing the Display

Modify the Starlark template in `SG_Data.py` to change the visualization - refer to Tidbyt documentation.

## Troubleshooting

Check the log files for errors:
- `/Applications/Tidbyt/com.Tidbyt.SG_Data.out` - Standard output
- `/Applications/Tidbyt/com.Tidbyt.SG_Data.err` - Error messages

Common issues:
- API key incorrect or expired
- Tidbyt device ID not found
- Network connectivity problems

## License

This project is licensed under the MIT License - see the LICENSE file for details.
