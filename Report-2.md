# Copy-Move Image Forgery Detection Using Sherloq

## Introduction
In the field of digital forensics, image manipulation poses a significant threat, particularly concerning the integrity and authenticity of visual evidence. One commonly employed manipulation technique is copy-move forgery, in which a portion of an image is copied and pasted into another area within the same image to conceal or alter information.

This case study utilizes Sherloq, an open-source forensic image analysis tool, to detect instances of copy-move forgery. Through experiments conducted on a minimum of five image samples, the study aims to analyze manipulation patterns and evaluate the detection accuracy achieved by Sherloq.

## Tools Used
In this case study, `Sherloq` was used as the primary tool for detecting copy-move forgeries within digital images.

## Digital Evidence Description
The digital evidence analyzed in this study consisted of six sample images suspected of containing copy-move forgeries. These images were selected from a provided dataset to evaluate the algorithm’s capability in identifying duplicated regions within an image. Each image was examined to determine the specific areas that had been copied and relocated within the same image.
<img width="600" alt="Sample1-3" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-1.png"><br>
<img width="600" alt="Sample4-6" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-2.png">

## Forensic Process
The following outlines the procedures performed during the image analysis process using Sherloq:
1. The selected image is imported into Sherloq for analysis.
2. Once loaded, the image is divided into multiple segments to facilitate comparison between different regions.
3. Each region is compared with others to identify areas exhibiting significant similarity. Sherloq supports several feature-matching algorithms for this process, including `BRISK`, `ORB`, and `AKAZE`.
4. When duplicated regions originating from copy-move operations are detected, Sherloq highlights and visualizes these areas.
5. The visualization results can be refined by adjusting several parameters, such as response, matching, distance, and cluster.

A series of six test cases were conducted using the Sherloq tool to evaluate the performance of different detection algorithms under varying conditions. Each test provided insight into the accuracy and reliability of the algorithms in identifying duplicated regions within manipulated images.

- **Test Case 1**<br>
<img width="800" alt="BRISK-1" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-3.png"><br>
<img width="800" alt="ORB-1" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-4.png"><br>
<img width="800" alt="AKAZE-1" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-5.png"><br>

- **Test Case 2**<br>
<img width="800" alt="BRISK-2" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-6.png"><br>
<img width="800" alt="ORB-2" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-7.png"><br>
<img width="800" alt="AKAZE-2" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-8.png"><br>

- **Test Case 3**<br>
<img width="800" alt="BRISK-3" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-9.png"><br>
<img width="800" alt="ORB-3" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-10.png"><br>
<img width="800" alt="AKAZE-3" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-11.png"><br>

- **Test Case 4**<br>
<img width="800" alt="BRISK-4" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-12.png"><br>
<img width="800" alt="ORB-4" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-13.png"><br>
<img width="800" alt="AKAZE-4" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-14.png"><br>

- **Test Case 5**<br>
<img width="800" alt="BRISK-5" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-15.png"><br>
<img width="800" alt="ORB-5" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-16.png"><br>
<img width="800" alt="AKAZE-5" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-17.png"><br>

- **Test Case 6**<br>
<img width="800" alt="BRISK-6" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-18.png"><br>
<img width="800" alt="ORB-6" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-19.png"><br>
<img width="800" alt="AKAZE-6" src="https://github.com/hanamahes78/Digital-Forensics/blob/main/img/R2-20.png"><br>

## Conclusion
Based on the experiments conducted, the BRISK algorithm demonstrated the most consistent and accurate performance in detecting copy-move forgeries. While ORB and AKAZE were also able to identify manipulations under certain parameter adjustments, both exhibited a higher probability of false detection compared to BRISK.

These findings indicate that Sherloq, when configured with the appropriate algorithm and parameter settings, serves as an effective tool for detecting and analyzing copy-move forgeries in digital images. This study also underscores the importance of selecting suitable algorithms and fine-tuning parameters to achieve optimal results in forensic image analysis.

## References

https://www.vcl.fer.hr/comofod/examples.html
