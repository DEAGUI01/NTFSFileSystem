<h1>NTFS File System Lab</h1>


<h2>Description</h2>
In this lab I investigate the NTFS File System, one of the most common file systems used by Windows. It includes the following tasks:
- <b>Examining the NTFS File System</b> 
- <b>Using a hex Editor to Explore an NTFS Partition</b>
- <b>Verifying and viewing forensic image details</b>
- <b>Analyzing an NTFS Partition with Autopsy</b>
<br />


<h2>Software and Utilities Used</h2>
- <b>HxD Hex Editor</b>
<br />
- <b>Autopsy Forensic Browser</b>

<h2>Environments Used </h2>
- <b>InfoSec Learning Labs</b> 
<br />
- <b>Windows 7 Pro</b> 
<br />
- <b>Windows 10 Pro</b>


<h2>Lab Walkthrough:</h2>

<p align="center">

Log into Windows <br/>
<img src="https://i.imgur.com/vVozoW5.png" height="80%" width="80%" alt="Log into Windows"/>
<br />
<br />
Got to computer, select properties of FAT32 drive, see that the security and quota tabs are missing drive<br/>
<img src="https://i.imgur.com/06QbcWf.png" height="80%" width="80%" alt="Investigate FAT32"/>
<br />
<br />
Select properties on the C drive, check that the Security (this is where Access Control Lists can be configured) and Quota tabs (this is where disk usage can be restricted for users) are both present <br/>
<img src="https://i.imgur.com/oSm8O8P.png" height="80%" width="80%" alt="check quota and security tabs"/>
<br />
<br />
On the command prompt, using an ADS (Alternative Data Stream), hide a text file inside another text file  <br/>
<img src="https://i.imgur.com/dy6jmhI.png" height="80%" width="80%" alt="hide file using ADS"/>
<br />
<br />
Check that the size of the file in which the ADS was put did not change, also see that the ADS cannot be seen on the directory<br/>
<img src="https://i.imgur.com/rcxI5DQ.png" height="80%" width="80%" alt="check ads"/>
<br />
<br />
Use the command dir /r to view all ADS files on the directory, check that the hidden file can now be seen <br/>
<img src="https://i.imgur.com/NGCfInn.png" height="80%" width="80%" alt="dir /r"/>
<br />
<br />
Use timestomp to set the MACE (modified, accessed, created, entry) attributes of the config.sys file to those of source file hi.txt <br/>
<img src="https://i.imgur.com/41NdDTI.png" height="80%" width="80%" alt="dir /r"/>
<br />
<br />
Make a directory called private<br/>
<img src="https://i.imgur.com/QtP7Vmh.png" height="80%" width="80%" alt="analyze first partition"/>
<br />
<br />
Create a file called SSN.txt that says 123-45-6789   <br/>
<img src="https://i.imgur.com/OXXsH46.png" height="80%" width="80%" alt="analyze next 3 partitions"/>
<br />
<br />
On Windows Explorer, select the private folder properties, go to advanced, and enable encrypt contents to secure data, apply the changes <br/>
<img src="https://i.imgur.com/q3vlpCU.png" height="80%" width="80%" alt="log into Kali Linux"/>
<br />
<br />
Create a user, check that the user exists and get information about it, then add it to the administrators group and verify that the user has been added <br/>
<img src="https://i.imgur.com/SX30nAC.png" height="80%" width="80%" alt="create admin user"/>
<br />
<br />
Log off and log in as the user previously created <br/>
<img src="https://i.imgur.com/dMAF8Zu.png" height="80%" width="80%" alt="check md5sum"/>
<br />
<br />
Go to the private folder and try to open the SSN.txt file, see that access is denied <br/>
<img src="https://i.imgur.com/m5VXSz0.png" height="80%" width="80%" alt="check sha1sum"/>
<br />
<br />
Back into Windows as the student user, open HxD Hex Editor and open the image of an NTFS partition <br/>
<img src="https://i.imgur.com/774tawn.png" height="80%" width="80%" alt="Autopsy"/>
<br />
<br />
Highlight bytes 00000000 to 00000162. This is a piece of the boot code for the drive that allows it to become bootable.
<img src="https://i.imgur.com/U6Eqk5F.png" height="80%" width="80%" alt="new case"/>
<br />
<br />
Highlight bytes 00000163 to 000001B2. This area is also part of the boot code and contains any error messages. The message can be seen on the right, missing operating system. <br/>
<img src="https://i.imgur.com/Mbbbexo.png" height="80%" width="80%" alt="add host"/>
<br />
<br />
The first partition of the disk begins at location 1BE to 1CD (446–461 in decimal) and is 16 bytes long.<br/>
<img src="https://i.imgur.com/gPZmCqw.png" height="80%" width="80%" alt="add image"/>
<br />
<br />
You can have up to four primary partitions on a standard DOS based system. There are three more partitions on this image. The second partition goes from 1CE to 1DD or from 462 to 477 decimal. The third partition is from 1DE to 1ED or from 478 to 493 in decimal. The fourth and last partition is from 1EE to 1FD or from 494 to 509 decimal. The entire partition table is 64 bytes in length. The first byte of the partition tells whether it is a bootable partition or not, 00 means is non bootable, 80 means it's bootable. The fifth byte is the Partition Type, in this case NTFS. The next three bytes indicate the ending CHS address. The next four bytes is for Logical Block Addressing (LBA). The Operating System determines the LBA. Possible choices are either CHS or LBA mode but not both for the partition. The last 4 bytes indicate the size in sectors of the partition. Finally, the MBR signature is at the end of the Master Root Record as highlighted below—55 AA. <br/>
<img src="https://i.imgur.com/h8b7phc.png" height="80%" width="80%" alt="analyze"/>
<br />
<br />
On Windows 10, under the properties of an image file, check that the file hashes match those found on the text file that accompanies the image <br/>
<img src="https://i.imgur.com/h8b7phc.png" height="80%" width="80%" alt="anylze files"/>
<br />
<br />
Create New Case on Autopsy<br/>
<img src="https://i.imgur.com/wyk69ft.png" height="80%" width="80%" alt="add image"/>
<br />
<br />
Select forensic image as data source<br/>
<img src="https://i.imgur.com/syNLO6i.png" height="80%" width="80%" alt="add image"/>
<br />
<br />
Open the image. Scroll down on the files and notice the NTFS system files including the Master File Table $MFT and the $MFTMirr.
<img src="https://i.imgur.com/nW1ppEh.png" height="80%" width="80%" alt="add image"/>

<h2>Conclusion </h2>
There are many variations of file systems that are used on operating systems. File Systems that are common to Microsoft operating systems include File Allocation Table (FAT) and New Technology File System (NTFS). Some of the features included with the NTFS file system include Alternate Data Streams (ADS) and the Encrypted File System (EFS). A hacker can also perform timestomping on an NTFS volume. A hexadecimal (hex) editor like HxD will allow you to examine the details of FAT or FAT32 Partitions and disk images. When an image is collected, the incident responder should generate a corresponding text file with the image MD5 and SHA1 hash values as well as other information such as the cyclical redundancy check (CRC value). The md5sum and sha1sum utilities can be utilized from the terminal to hash a data set to verify the integrity of the data. Autopsy is a forensic analysis tool that is free to use. Commercial forensic products, such as EnCase and FTK, are more widely used but are not free and require hardware dongles. All of these products allow you to parse fornesic images and examine files.
</p>
