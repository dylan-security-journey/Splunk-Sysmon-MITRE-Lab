# Splunk Enterprise Troubleshooting Log

This file documents the issues I encountered while setting up and running **Splunk Enterprise** with Sysmon data, and how I solved them.  
The goal is to track fixes, understand root causes, and have quick reference notes for future projects.

---

## Issue 1 – Sysmon Data Not Parsing Correctly
**Date:** Sept 4, 2025  
**Project:** Splunk Enterprise + Sysmon Integration  

- **Error/Symptom:** Sysmon events were being ingested, but they weren’t readable/structured in Splunk.  
- **Cause:** Splunk was receiving XML-formatted Sysmon logs but didn’t know how to parse them without the correct add-on.  
- **Solution:** Installed the **Splunk Add-on for Microsoft Sysmon** so Splunk could properly interpret the XML event structure.  
- **Lesson Learned:** Always check if Splunk needs an **add-on/TA (Technology Add-on)** to understand the event source.

---

## Issue 2 – Parse Issue with inputs.conf
**Date:** Sept 4, 2025  
**Project:** Splunk Enterprise + Sysmon Integration  

- **Error/Symptom:** Data was flowing, but Splunk didn’t recognize the source type correctly.  
- **Cause:** The `inputs.conf` file didn’t specify the correct sourcetype for Sysmon logs.  
- **Solution:** Updated the sourcetype in `inputs.conf` to:  
