# Usage Guide: Niagara BACnet Topology Builder

## 🚀 Quick Start

### 1. Download
Download the latest executable from the [releases page](../releases/v1.0.0/NiagaraBACnetTopologyBuilder.exe).

### 2. Run
Double-click the executable file. No installation is required.

### 3. Import Excel File
Click "Browse Excel..." and select your BACnet points Excel file.

### 4. Configure Settings
- **Topology**: Choose 2-Layer (JACE + Main) or 3-Layer (JACE + Zone + Main)
- **Default Zone Server**: Select ZONEA, ZONEB, or ZONEC
- **Default JACE Name**: Enter your JACE controller name (default: JACE_1)
- **JACE Device Format**: Choose how device names are formatted (default: dev<Instance>)

### 5. Validate & Build
Click "✓ Validate & Build" to process your Excel file.

### 6. Review & Export
- Review the generated XML in the preview pane
- Check the tree views for each layer
- Export individual layers or combined BOG file

---

## 📋 Excel File Format

### Required Columns
Your Excel file must have a sheet named `BACnet_Points` with these required columns:

| Column | Description | Example |
|--------|-------------|---------|
| Controller ID | Numeric BACnet device instance | 214014 |
| Point Description | Human-readable point name | Supply Air Temperature |
| Point Type | BACnet point type | AI |
| Point Number | Numeric BACnet object instance | 1 |

### Optional Columns
| Column | Description | Example |
|--------|-------------|---------|
| Zone Name / Zone | Zone identifier | Zone_B, ZONEB, B |
| JACE Name | JACE/controller name | P214_J1 |
| Parcel | Parcel/site identifier | 214 |
| CBMS Name | Equipment/system folder name | AHU01 |
| Analog Point Unit | Unit of measurement | DegC, %, kPa |
| Active Usage | Active state text | On, Open, Active |
| Inactive Usage | Inactive state text | Off, Closed, Inactive |

### Point Type Values
Use these exact values for the Point Type column:
- **AI** - Analog Input
- **AO** - Analog Output
- **AV** - Analog Value
- **BI** - Binary Input
- **BO** - Binary Output
- **BV** - Binary Value
- **MSI** - Multi-State Input
- **MSO** - Multi-State Output
- **MSV** - Multi-State Value

---

## 🎯 Topology Options

### 2-Layer Topology (JACE + Main)
- **Layer 1**: JACE → BacnetNetwork → dev_<Instance> → points → folder
- **Layer 2**: Main → NiagaraNetwork → <JACE> → points → folder

Use this for projects without a zone server layer.

### 3-Layer Topology (JACE + Zone + Main)
- **Layer 1**: JACE → BacnetNetwork → dev_<Instance> → points → folder
- **Layer 2**: Zone → NiagaraNetwork → <JACE> → points → folder
- **Layer 3**: Main → NiagaraNetwork → <ZONE> → points → folder

Use this for projects with a zone server layer.

---

## 🏗️ Zone Configuration

### Zone Naming
All zone names are automatically normalized to:
- **ZONEA** (for A, ZA, Zone_A, Zone A, etc.)
- **ZONEB** (for B, ZB, Zone_B, Zone B, etc.)
- **ZONEC** (for C, ZC, Zone_C, Zone C, etc.)

### Default Zone
Set the default zone server in the UI. This will be used for any points that don't have a zone specified in the Excel file.

---

## 🔧 JACE Device Naming

Choose how BACnet device names are formatted:

| Format | Example | Description |
|--------|---------|-------------|
| `dev<Instance>` | dev1, dev2 | Default format |
| `dev_<Instance>` | dev_1, dev_2 | With underscore |
| `<CustomPrefix><Instance>` | mydev1, mydev2 | Custom prefix |
| `Device_<Instance>` | Device1, Device2 | Full word |
| `<Instance> Only` | 1, 2 | Just the number |
| `<CBMS_Name>_<Instance>` | AHU01_1, AHU01_2 | CBMS name prefix |

---

## 📤 Export Options

### Export All Layers (Single BOG)
Exports all layers (JACE, Zone, Main) in a single .bog file.
- Filename: `BACnet_Combined_3_Layers_{zone}_{jace}_{cbms}.bog` (3-layer)
- Filename: `BACnet_Combined_2_Layers_{jace}_{cbms}.bog` (2-layer)

### Export Individual Layers
- **Layer 1 (JACE)**: JACE BacnetNetwork configuration
  - Filename: `1_JACE_{jace}_{cbms}_BacnetNetwork.bog`
- **Layer 2 (Zone)**: Zone server proxy points (3-layer only)
  - Filename: `2_{zone}_{jace}_{cbms}_NiagaraNetwork.bog`
- **Layer 3 (Main)**: Main CBMS station configurations
  - Filename: `3_Main_{zone}_{cbms}_NiagaraNetwork.bog` (3-layer)
  - Filename: `2_Main_{jace}_{cbms}_NiagaraNetwork.bog` (2-layer)

---

## 🔍 Validation & Error Handling

### Validation Checks
The tool validates:
- ✅ Required columns are present
- ✅ Controller IDs are numeric
- ✅ Point types are valid
- ✅ Point numbers are numeric
- ✅ Zone names are valid (normalized to ZONEA/ZONEB/ZONEC)

### Error Report
If validation fails:
- Errors are displayed in the "Validation Issues" tab
- A detailed report can be exported as Excel
- Each error shows: row number, column, error type, description

---

## 💡 Tips & Best Practices

### 1. Start Small
Begin with a small Excel file (10-20 points) to test the configuration before processing large files.

### 2. Use Consistent Naming
Use consistent naming conventions for:
- Controller IDs
- Point descriptions
- Zone names
- JACE names
- CBMS names

### 3. Validate First
Always click "Validate & Build" before exporting to catch any issues early.

### 4. Check the Preview
Review the generated XML in the preview pane before exporting.

### 5. Test in Niagara
Always test the imported BOG file in a development Niagara station before deploying to production.

---

## ❓ Troubleshooting

### Common Issues

**Issue: "Invalid Controller ID"**
- **Cause**: Controller ID is not numeric
- **Solution**: Ensure Controller ID column contains only numbers

**Issue: "Invalid Point Type"**
- **Cause**: Point type is not one of the valid values
- **Solution**: Use only: AI, AO, AV, BI, BO, BV, MSI, MSO, MSV

**Issue: "Missing required columns"**
- **Cause**: Excel file is missing required columns
- **Solution**: Add Controller ID, Point Description, Point Type, and Point Number columns

**Issue: "CannotLoadBogException" in Niagara**
- **Cause**: Invalid BOG file format
- **Solution**: Check the validation report and ensure all data is correct

### Getting Help
For issues not covered here:
- Check the validation report for specific errors
- Review your Excel file format
- Contact support: mdkamalhosennn@gmail.com

---

## 📚 Example Excel File

Here's a simple example of what your Excel file should look like:

| Controller ID | Point Description | Point Type | Point Number | Zone Name | JACE Name | CBMS Name | Analog Point Unit |
|---------------|-------------------|------------|--------------|-----------|-----------|-----------|-------------------|
| 214014 | Supply Air Temp | AI | 1 | Zone_B | P214_J1 | AHU01 | DegC |
| 214014 | Return Air Temp | AI | 2 | Zone_B | P214_J1 | AHU01 | DegC |
| 214014 | Supply Fan Status | BI | 1 | Zone_B | P214_J1 | AHU01 | - |
| 214014 | Supply Fan Command | BO | 1 | Zone_B | P214_J1 | AHU01 | - |

This will generate a complete BACnet topology configuration with:
- 1 BACnet device (214014)
- 4 points
- Zone normalized to ZONEB
- JACE: P214_J1
- CBMS: AHU01

---

## 🎯 Advanced Features

### Alarm Configuration
All points with `_sp` or `_cmd` in the name and containing "Alarm" or "Achieved" or "Mismatch" will have alarm configuration added automatically.

### History Configuration
Points can have history configuration based on their type and naming.

### Device Info Points
Each BACnet device folder automatically includes:
- Device_Name
- Device_Id
- Device_Status

These points are visible in wire sheets and property sheets but not in the BACnet point list.

---

**Developed by**: Md Kamal Hosen  
**Version**: 1.0.0  
**Last Updated**: November 11, 2026
