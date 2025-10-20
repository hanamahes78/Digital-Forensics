# Forensic Analysis of Windows System Image

## Introduction
A Windows forensic image was provided for examination. The task of this investigation is to perform a forensic analysis on the image and get informations to uncover system details and potential evidence of activity within the Windows environment.

## Tools Used
The tools utilized in this investigation are `FTK Imager`, which was used to acquire, verify, and examine the forensic image as well as retrieve metadata and file properties, and `Autopsy` used for in-depth forensic analysis, including registry parsing and evidence extraction.

## Digital Evidence Description
The digital evidence consists of a Windows forensic image with a total size of 10.62 GB. This image contains system and user-level data required for forensic examination.

## Forensic Process
1. **Evidence Acquisition**<br>
The forensic image was obtained from the following source:<br>
[Forensic Image](https://drive.google.com/drive/folders/16dWkoiQv9fzLlIa4uo8tF3HmS47Tai2W)

2. **Adding the Evidence to FTK Imager**<br>
The acquired forensic image was loaded into FTK Imager as an evidence item for detailed examination and metadata extraction.<br>
<img width="800" alt="Process-2" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-1.png"><br>

## Findings
1. **Image SHA1 Hash**<br>
The SHA1 hash of the forensic image was obtained through the Properties tab in FTK Imager. The calculated SHA1 hash value ensures the integrity and authenticity of the forensic image.<br>
<img width="600" alt="Finding-1" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-2.png"><br>

2. **Who Performed the Forensic Image Acquisition**<br>
Based on the metadata information available in the Properties section, the recorded examiner name is "**Powers**".<br>
<img width="600" alt="Finding-2" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-3.png"><br>

3. **Volume Serial Number of the Operating System**<br>
From the image metadata under the `Properties → File System Information section`, the Volume Serial Number was identified as `CCEE-841B`.<br>
<img width="600" alt="Finding-3" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-4.png"><br>

4. **Image Timezone Setting**<br>
The system timezone was determined by analyzing the SYSTEM registry hive configuration file. The image was configured to use the `SE Asia Standard Time` timezone.<br>
<img width="600" alt="Finding-4" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-5.png"><br>

5. **Timezone During Image Acquisition**<br>
Metadata associated with the image indicates that the timezone during acquisition was `Asia/Jakarta`.<br>
<img width="600" alt="Finding-5" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-6.png"><br>

6. **Windows Operating System Installation Date**<br>
The installation date was identified by examining the registry key `SOFTWARE\Microsoft\Windows NT\CurrentVersion`.<br>
<img width="600" alt="Finding-6a" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-7.png"><br>
The **InstallDate** value, stored in UNIX Epoch Time, was converted to reveal that Windows was installed on `July 28, 2018`.<br>
<img width="800" alt="Finding-6b" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-8.png"><br>

7. **MFT record Number Associated with File `\Users\Administrator\Desktop\FTK_Imager_Lite_3.1.1\FTK Imager.exe`**<br>
The MFT record number can be found in the NTFS Information section of the file’s properties in FTK Imager.<br>
<img width="600" alt="Finding-7a" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-9.png"><br>
The following MFT record number was identified.<br>
<img width="800" alt="Finding-7b" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-10.png"><br>

8. **Desktop IP Address**<br>
By analyzing the registry path `SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces\`,
the system’s configured IP address was identified as `64.44.141.76`.<br>
<img width="600" alt="Finding-8" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R6-11.png"><br>

## Conclusion
The forensic examination successfully extracted key system configuration details and metadata from the provided Windows forensic image using FTK Imager and Autopsy. Through the analysis, essential information such as the image integrity (SHA1 hash), examiner identity, timezone settings, system installation date, and IP configuration were determined.


This case highlights the effectiveness of digital forensic tools in validating image integrity, retrieving system configuration data, and uncovering timeline and usage patterns from a forensic image.
