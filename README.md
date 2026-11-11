# Niagara BACnet Topology Builder

A professional tool for building Niagara 4 BACnet topology configurations from Excel spreadsheets. Supports both 2-layer (JACE + Main) and 3-layer (JACE + Zone + Main) architectures.

## 📥 Download

### Latest Release: v1.0.0

Download the executable:
- [NiagaraBACnetTopologyBuilder.exe](releases/v1.0.0/NiagaraBACnetTopologyBuilder.exe) (18MB)

## ✨ Features

- **Multi-Layer Support**: Generate configurations for 2-layer or 3-layer BACnet topologies
- **Excel Import**: Import BACnet point lists from Excel workbooks
- **Zone Normalization**: Automatically normalizes zone names to ZONEA, ZONEB, ZONEC format
- **Device Info Points**: Automatically adds Device_Name, Device_Id, and Device_Status points
- **Alarm Configuration**: Built-in alarm configuration with proper Niagara 4 format
- **History Configuration**: Built-in history configuration for trend logging
- **Proxy Points**: Creates read-only proxy points that work with Niagara 4
- **Validation**: Validates input data and provides detailed error reports
- **Multiple Export Options**: Export individual layers or combined BOG files

## 📋 Requirements

- **Operating System**: Windows 10 or 11
- **Microsoft Excel**: For creating input files
- **Niagara 4**: For importing BOG files

## 💡 Usage

### Step 1: Prepare Your Excel File

Create an Excel workbook with a sheet named `BACnet_Points` containing the following columns:

**Required Columns:**
- Controller ID (numeric BACnet device instance)
- Point Description (human-readable point name)
- Point Type (AI, AO, AV, BI, BO, BV, MSI, MSO, MSV)
- Point Number (numeric BACnet object instance)

**Optional Columns:**
- Zone Name / Zone (will be normalized to ZONEA, ZONEB, or ZONEC)
- JACE Name (JACE/controller name)
- Parcel (parcel/site identifier)
- CBMS Name (equipment/system folder name)
- Analog Point Unit (unit of measurement)
- Active Usage (active state text)
- Inactive Usage (inactive state text)

### Step 2: Run the Application

1. Download the executable from the [releases page](releases/v1.0.0/NiagaraBACnetTopologyBuilder.exe)
2. Run the executable (no installation required)
3. Click "Browse Excel..." to select your Excel file
4. Choose the project topology (2-Layer or 3-Layer)
5. Set the default zone server (ZONEA, ZONEB, or ZONEC)
6. Configure JACE device naming format
7. Click "✓ Validate & Build" to process the file
8. Review the generated XML in the preview pane
9. Export individual layers or combined BOG file

## 📊 Zone Naming Convention

This tool normalizes all zone names to the following format:
- **ZONEA** (for Zone A, Zone_A, ZA, A, etc.)
- **ZONEB** (for Zone B, Zone_B, ZB, B, etc.)
- **ZONEC** (for Zone C, Zone_C, ZC, C, etc.)

## 🎯 Device Naming Options

- `dev<Instance>` - Default format (e.g., dev1, dev2)
- `dev_<Instance>` - With underscore (e.g., dev_1, dev_2)
- `<CustomPrefix><Instance>` - Custom prefix from settings
- `Device_<Instance>` - Full word format
- `<Instance> Only` - Just the instance number
- `<CBMS_Name>_<Instance>` - CBMS name prefix

## 📝 Alarm Configuration

Alarms are configured with the following defaults:
- **Alarm Delay**: 1 minute (60000ms)
- **Alarm Enable**: toOffnormal only
- **Alarm Source Name**: `%parent.parent.parent.parent.displayName%_%parent.parent.displayName%_%parent.displayName%`
- **To Offnormal Text**: `%alarmData.sourceName% Is Offline`
- **To Normal Text**: `%alarmData.sourceName% has returned to Normal`

## 🏷️ Alarm Classes

Each building/parcel has 3 alarm classes:
- `P214_MAJOR`
- `P214_CRITICAL`
- `P214_MINOR`

## 🔄 Reset Points

Reset points (alarm) are configured as:
- Read-only points without AlarmSourceExt
- No alarm or trend configuration
- All reset points follow the same pattern

## 📚 Documentation

- [Full Documentation](docs/) (coming soon)
- [Usage Guide](docs/usage-guide.md) (coming soon)

## 📧 Support

For support, questions, or bug reports:
- **Email**: mdkamalhosennn@gmail.com
- **LinkedIn**: https://www.linkedin.com/in/mdkamalhosen
- **Website**: https://mdkamalhosen.com

## 📜 License

This software is proprietary. All rights reserved.

## 🏗️ Technical Details

### Niagara 4 Compatibility

- Uses proper module declarations (m="a=alarm", m="h=history", m="nd=niagaraDriver", m="bac=bacnet")
- Proxy points are read-only types (BooleanPoint, NumericPoint, StringPoint)
- Includes writeValue and VirtualAction elements for writability
- Proper unit facet encodings matching production format

### BOG File Format

- Generates valid Niagara 4 BOG XML files
- Can be saved as .bog (compressed) or .xml (uncompressed)
- Compatible with Niagara 4 import functionality

---

**Note**: This is a Windows application. The source code is private.

**Developed by**: Md Kamal Hosen
