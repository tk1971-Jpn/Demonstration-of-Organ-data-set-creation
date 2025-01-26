# Demonstration-of-Organ-data-set-creation
## example-organ-data-set-creation
This document explains the procedure for importing and displaying contrast-enhanced CT MPR images in Blender, as well as editing organ surface data in STL format that was automatically extracted using 3D Slicer. A demonstration of this workflow will be conducted using data obtained from TCIA (The Cancer Imaging Archive).

**Prepare**  
To perform this task, please prepare the following software on your PC.  
This document explains the workflow on a Mac (Apple Silicon).  

**Python**:
https://www.python.org  
**Blender**:
https://www.blender.org  
**3D Slicer**:
https://www.slicer.org  
**NBIA Data Retriever**:
https://wiki.cancerimagingarchive.net/display/NBIA/Downloading+TCIA+Images#DownloadingTCIAImages-DownloadingtheNBIADataRetriever  

**Data for Demonstration**  

Access TCIA and go to the Search Radiology page　https://www.cancerimagingarchive.net/access-data/. Under Collections, select **"TCGA-STAD"**. From the list, choose the subject ID **"TCGA-VQ-AA6B"**. Among the seven series, select the data with the description **"CC1.25mm"**.  
Click the **Cart** button at the top of the screen and proceed with the download. This will download a file named **manifest-numbers.tcia**. When you open this file, the pre-installed **NBIA Data Retriever** will launch. Specify the directory where you want to save the data, and click the **Start** button at the bottom left to begin downloading the DICOM data.

**1. Converting DICOM data to MPR images in JPEG format**  

1. Download the Python script from [https://github.com/tk1971-Jpn/DICOM-to-JPEG](https://github.com/tk1971-Jpn/DICOM-to-JPEG) and run it on Jupyter Lab.  
2. Assign the path of the first file in the DICOM data to `path_ct`. Specify the directory to save the generated MPR images in `storage_path`.  

<img width="612" alt="set file path" src="https://github.com/user-attachments/assets/e62bc9c3-328f-40cf-bfad-60c9b696c21c" />

3. Run cells 1 to 3 and take note of the information obtained along the way.

<img width="612" alt="take note the of imformation" src="https://github.com/user-attachments/assets/4a3600bd-c032-43aa-ab5d-96f4da18bc60" />

4. Run the fourth cell and adjust the values of `k` and `l` using the sliders to obtain the desired image.　　

<img width="612" alt="Window setting" src="https://github.com/user-attachments/assets/91692c4d-d9ae-4b11-91e1-776122f6d4d6" />

5. Take the values of `k` and `l` obtained from the fourth cell and substitute them into the fifth cell as `Vmin = l - k` and `Vmax = l + k`.　(In this demonstration, `k` was set to 320 and `l` to 1080, resulting in `Vmin` being 760 and `Vmax` being 1400.)  
When you run the cell, new folders named `A`, `C`, and `S` will be created in the specified folder, and the generated JPEG files will be saved inside them.


**2. Obtaining organ surface data in STL format from DICOM data using 3D Slicer**  

1. Launch 3D Slicer.
2. In the `Add DICOM Data` module, click the `Import DICOM Files` button, select the folder containing the DICOM files, and import the DICOM data.
3. In the `Add DICOM Data` module, click the `Show DICOM database` button, select the imported data, and click the `Load` button at the bottom of the screen.
Click the arrow-like button at the top-right corner of each **R**, **G**, and **Y** window. A closed eye icon will appear; click it to open the eye, and the CT image will be displayed in the central window. You can move the sliders in the **R**, **G**, and **Y** windows to display the desired CT image.

<img width="612" alt="slicer" src="https://github.com/user-attachments/assets/c9ca0ad7-38c5-4914-9ff0-f3fe549f7c41" />

4. In the `Segmentation` module, select `Total Segmentator` and click the `Apply` button. You can choose between `Full-Resolution` and `Fast`. Here, select `Fast`.　The organ segmentation is completed in about 2 minutes. Click the `Show 3D` button to display the organ surface data in the central window.　

<img width="612" alt="segmented" src="https://github.com/user-attachments/assets/aadfbfef-adb2-47d7-93c3-ae81a50448d1" />

5. In the `Segmentation` module, click the `*Export to files` button (not the **Export** button). Specify the folder to save the files in the `Destination Folder` field, then click the `Export` button within the `Export to files` tab. All segmented organ data in STL format will be saved in the specified folder.


**3. Displaying MPR Images in Blender Using Sliders**

1. Launch Blender
2. Select the Scripting mode and click the "New Text" button (the icon with two overlapping documents) to enable the creation of a new script.
3. Copy the script from the file "Slider on Blender (foot to head > axial) for Blender ver 4.3.2.ipynb" available at [https://github.com/tk1971-Jpn/Slider-viewer-in-Blender](https://github.com/tk1971-Jpn/Slider-viewer-in-Blender) and paste it into Blender.
4. Substitute the values obtained in steps 1-3 into the script using the following assignments:

- `pixel_pitch`: The second value of "pixel pitch"  
- `pixel`: The third value of "size of 3D array"  
- `ax_slide_number`: The value of "Number of files"  
- `ax_slide_distance`: The value of "slice distance"

5. Assign the directory of the folder where the MPR images are stored (containing subfolders named A, C, and S) to `path_JPEG`.
6. When you run the script, an "Image Slider" tab will appear in the sidebar of the 3D viewport, allowing you to move the sliders to display the desired image in each direction.

<img width="612" alt="slider" src="https://github.com/user-attachments/assets/2fb91867-bbdf-4f61-89af-1da5f7777a51" />

  
**4. Importing Organ STL Data into Blenders**

1. Create another text data block.
2. Copy the script from the "import organ STL ver3" file available at [https://github.com/tk1971-Jpn/Import-organ-STL-data-into-Blender](https://github.com/tk1971-Jpn/Import-organ-STL-data-into-Blender) and paste it into Blender's text data block.
3. When you run the script, the organ's STL data will be imported.　However, the default movement is set to 0, causing a misalignment with the CT.　In most cases, the misalignment occurs only along the Y-axis.
4. Select an organ, such as the liver, which is easier to adjust, and move it to correct its position. Record the movement distances.
5. After that, delete all imported STL data (press **A** to select all and then press **X** to delete everything). Assign the recorded movement distances to `x_move`, `y_move`, and `z_move` in the script, and run the script again. This will place all organs in their correct positions.

<img width="612" alt="imported organs" src="https://github.com/user-attachments/assets/f894727f-123b-4436-832c-27e704d8bdda" />









