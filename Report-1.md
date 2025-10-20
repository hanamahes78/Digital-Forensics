# File Recovery from Flash Drive

## Introduction
In the digital era, portable storage devices such as flash drives are widely utilized for data storage and transfer. However, the risk of data loss has also increased, whether due to intentional or unintentional actions. One of the most common incidents involves the permanent deletion of files.

This case study focuses on the recovery of permanently deleted files from a flash drive using FTK Imager and Autopsy. This report provides a detailed description of each procedure undertaken throughout the investigation, including image acquisition, analysis, and file recovery. The objective of this investigation is to demonstrate that permanently deleted data can be successfully recovered through the application of appropriate forensic methodologies and tools.

## Tools Used
In this study, `FTK Imager` was used to create a forensic image of the flash drive, ensuring that a complete digital copy of the device was generated to maintain data integrity during analysis. After that, `Autopsy` was used to examine the image and recover deleted files. Autopsy also facilitates the extraction of information related to file activities and metadata within the flash drive.

## Digital Evidence Description
The digital evidence analyzed in this study consisted of a flash drive from which several files had been permanently deleted.

## Forensic Process
1. **Data Acquisition Using FTK Imager** <br>
FTK Imager was utilized to acquire a disk image of the flash drive. The appropriate drive was selected to ensure a complete duplication of the data contained within the device.
<img width="600" alt="Select Drive" src="">

2. **Creation of the Disk Image** <br>
The image type was specified as .dd format. Relevant metadata, including image name, description, and date, was recorded prior to initiating the imaging process.
<img width="600" alt="Create Image" src="">

3. **Flash Drive Imaging Results** <br>
Upon completion of the imaging process, a digital image of the flash drive was successfully generated. This image represents an exact, bit-by-bit copy of the original data.
<img width="600" alt="Creating Image" src="">
<img width="600" alt="Image Created" src="">

4. **Mounting the Image** <br>
The acquired image was mounted to enable secure access and examination of its contents without altering the original data.
<img width="600" alt="Mount Image" src="">

5. **Results of Image Mounting** <br>
Following the mounting process, the contents of the flash drive image became accessible for detailed inspection and further forensic analysis.
<img width="600" alt="Image in Drive" src="">

6. **File Recovery Using Autopsy** <br>
The Autopsy tool was employed to analyze the mounted image and recover deleted files. The identified deleted files were extracted and restored to a designated location for verification and documentation.
<img width="600" alt="Extract File" src="">

7. **Recovered Files** <br>
The files that had been permanently deleted were successfully recovered and stored within a separate recovery folder for further examination.
<img width="600" alt="Recovered File" src="">

## Conclusion
This case study demonstrates that the recovery of permanently deleted files from a flash drive can be effectively conducted through the use of FTK Imager and Autopsy. By creating a forensic image, an exact digital replica of the flash drive was obtained, allowing comprehensive analysis while preserving the integrity of the original evidence.

The recovered files, along with their associated metadata, confirm that even data deleted permanently can still be restored using appropriate forensic tools and techniques.
This finding point the importance of forensic imaging and analytical tools in digital investigations aimed at preserving and recovering critical evidence.

## References
 https://cloudfront.net/Imager/4_7_1/FTKImager_UserGuide.pdf
 https://www.ubackup.com/data-recovery-disk