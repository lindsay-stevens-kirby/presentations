# Tablet Data Collection with ODK Collect
Lindsay Stevens
2016-10-03



# Topics
- Introductions
- Overview of the XForms universe
- Our Use Case
- Development process
- Deployment process



# Introductions


## About Lindsay
- Education
  + BSc, Anatomy and Physiology
  + Working on Masters of Biostatistics
- University of Sydney: Cancer research
  + Project management ~ 4 years
  + Data systems ~ 1 year
- UNSW Australia: Hepatitis C research
  + Data systems ~ 3 years


## About the KI VHCRP
- Viral Hepatitis Clinical Research Program,
- VHCRP is part of the Kirby Institute, which is home to research specialists 
    in HIV, HCV and other infectious diseases,
- KI is part of the Faculty of Medicine at the University of New South Wales 
    (UNSW Australia)



# XForms 101


## What is an XForm
- XHTML document defining a form data model, behaviour and appearance.
- Responses are called "instances": copy of the data model populated with data.
- Main differences in HTML5 forms:
    - XForms defines the above in separate parts of the document,
    - HTML5 defines them together and/or uses JavaScript to help.


## XForm Structure: Header
- head
    - model
        - itext (text / media translations for questions, options)
        - instance \[main\] (data model of form: groups of XML elements)
        - instance \[static\] (additional XML e.g. for cascading select items)
        - bind (item attributes: data type, requiredness, conditional display, 
            validation constraints)


## XForm Structure: Body
- body (display controls for form items, for example:)
    - group
        - input
        - select1
    - group
        - geopoint
        - image
        - etc.



# XForms Universe


## The ODK Galaxy
- Many XForms implementations, but today focussing on JavaRosa / Open Data Kit 
    related tools.
- Components:
    - Form design
    - Form completion
    - Data processing
- Lots of options within each component, tailored to particular use cases, 
    preferences and requirements.


## XForm design
Options include:

- XLSForm spreadsheet -> convert to XForm
- Kobotoolbox Kpi web app -> export to XForm
- Write XHTML by hand -> sadness


## XForm completion
Options include:

- JavaRosa: J2ME / older / basic phones,
- ODK Collect: Android,
- Enketo: modern browsers
    - Uses XForm as a data definition and transport standard.
    - Converts XForm into nice looking HTML5 / Javascript page.


## XForm Data aggregation
Options include:

- ODK Aggregate: Java web app
- Kobocat: Python web app
- Manual transfer of XML from devices.


## Example Workflows: Newer
- KoBo: 
    - design: Kpi,
    - deploy: Enketo Express,
    - aggregate: Kobocat
- Enketo:
    - design: XLSForm
    - deploy: Enketo Express
    - aggregate: ODK Aggregate


## Example Workflows: Older
- ODK:
    - design: XLSForm
    - deploy: ODK Collect
    - aggregate: ODK Aggregate
- Offline:
    - design: manual
    - deploy: ODK Collect
    - aggregate: manual (XML files via email / Google Drive), process with ODK 
        Briefcase, XSLT, etc.



# What about OpenClinica?


## As a Data Warehouse
Treat aggregated XForm data like any other source (lab data, MedDRA coded AEs, 
etc.)

- Export data as files or obtain via web APIs etc,
- Define mappings to Events and CRFs (in code, DataUploader, ODIN, etc.),
- Upload data via SOAP / REST APIs.


## As an XForms Hub
Roll your own OpenClinica Participate (or sign up for it)

- design: OpenClinica
- deploy: Enketo Express
- aggregate: OpenClinica


## XForms Support in OpenClinica
- APIs for form retrieval, data submission, etc.
- Support for form definition as XForms, although with a (growing) subset of 
    features:
    - Standardise on OC: use OC CRF templates
    - Standardise on XForm: use XLSForm, Kpi, etc.,
- All signs point to this support increasing.
- Covered in more detail in other sessions.



# Use Case at VHCRP


## Problem
- So many options
- So little time
- What to do?


## Requirements
- Interest in offline electronic forms:
    - KI had previously used Nova QDS for it's audio guided forms capability (
        ODK supports text, images, audio, video).
    - Data quality gains vs. paper collection and later data entry.
- Nova QDS was Windows only (now has web), touch PCs were expensive and bulky.
- Initially required about 5 tablets for use at Hep-C diagnosis / liver health 
    campaign events at clinics in Australia.


## Constraints
- Dubious Internet availability at clinics: 
    - Poor reception or not allowed to connect external devices,
    - Logistical issues in providing mobile data, especially when expanding to 
        more sites, users, countries.
    - Mobile data not foolproof either: sim card - stolen! data quota - 
        exhausted! reception - sketchy!
- Standardised on assuming no Internet connectivity for devices.
    - Possibly to be reviewed once other aspects of the pipeline are sorted.


## Approach
- ODK Collect offline capable and works on cheaper Android devices.
- Data gathered from tablets via PC connection over USB or Bluetooth.
- Data submitted via email (...), or UNSW secure cloud storage platform.
- XML data aggregated with XSLT / Python for analysis in Stata or upload to 
    OpenClinica.



# Development


## Study Design
- Defined in the OpenClinica study
- Study system documentation describes events and forms
- Participant questionnaires implemented in ODK Collect
- A plain (no skips, validation, etc) version uploaded to OpenClinica


## Form Design
- XLSForm spreadsheet format defines the form structure
- "Survey" sheet: one row per item
- Item groups are defined in rows, one for group start, one for end,
- "Choices" sheet: all the drop down code lists and labels.


## Form Conversion
- PyXForm library converts XLSForm to XForm XML
- Also runs XForm result through ODK Validate
- Available as: online service, python source code


## Customised Converter
- Packages python code, ODK validate into a Windows GUI
- Add-on features for:
    - Generating question text as images for layout control,
    - Re-packing XForms as per-site editions with only their 
        local languages included.
- Available at: github.com/lindsay-stevens/odk_tools



# Deployment


## Device
- Selection
- Management
- Configuration


## Device Selection
Tenets:
- Data should be protected / difficult to retrieve by unauthorised users if 
    the device is stolen.
- Device should be a comfortable size: approx A4 to A5 / 8 to 10 inch.
- Some studies also using electronic blister packs for treatment adherence 
    so require a NFC reader.


## Devices Selected
So far:
- 46 HTC Nexus 9: Android 6, NFC-capable
- 36 Samsung Galaxy Tab varieties: Android 4.4 to 5.1
- 1 Lenovo IdeaTab?? Android 4.4


## Device Management: Platforms
Options include:
- Cisco Meraki
- Google Apps / Suite for Work (paid) / for Education (free)
- Spreadsheet / hope for the best


## Device Management: Capabilities
- Meraki / Google can enforce some settings / apps / restrictions
- Most of these settings are security related, some are only available in USA.
- Most configuration done manually by following a checklist.


### Device Configuration: Part 1
- Create Google account(s) for OS updates and Play store app access.
- Associate tablet with account
    - Android 6: Google account required after reset (to spite thieves)
- Activate device encryption (default on Android 6)


### Device Configuration: Part 2
- Set good password for "Administrator" / device owner user:
    - Can turn on / decrypt the tablet
    - Can add/remove accounts, create device users, adjust settings.
- Create Android "restricted account" user:
    - Access only to selected apps
    - Device storage separated from the Admin user.


### Device Configuration: Part 3
- Configure ODK Collect app and export settings file.
    - Or use collect_settings tool: github.com/lindsay-stevens/collect_settings.
- Copy XForm XML files, media, settings to the device for ODK Collect.
- Ship it!
- Before first real participant, have end users complete a test run of data 
    collection, retrieval from the device, submission to project team.



# Summary
- Mobile devices present a great opportunity for increasing data quality,
- Lots of flexibility in available tools to meet complex requirements,
- Easy mode: participants can bring their own device, everyone has 
    Internet, OpenClinica Participate or similar setup for data handling.
- Hard mode: what we did!

