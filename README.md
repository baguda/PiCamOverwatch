# PiCamOverwatch

PiCam Overwatch records videos video via the raspberry pi's camera module when it detects motion. 

Prep and Installation:

Required Hardware:
 Raspberry Pi 3 with 8GB+ SD card and needed accessories
 Raspberry Pi Camera Module 
 USB flash drive, 8GB+ (the bigger the better)

Required Software:
 Raspbian 8 (Jessie) 

Installation Steps:

1)Make sure your image is freash:

$ sudo apt-get upgrade

and:

$ sudo apt-get update

2)Install and enable Pi Camera

sudo rspi-config

>Interfacing Options
>>P1) Camera

3)Install dependancies, program needs MP4Box to function:

$ sudo apt-get install gpac

4)Download PiCamOverwatch install script:

$ git clone http://github.com/baguda/PiCamOverwatch.git

5)Navigate to PiCamOverwatch folder:

$ cd /home/pi/PiCamOverwatch/

6)Install PiCamOverwatch:

$ sudo python install

7)Input flash drive filepath into the installer's entry box and press install
  -open file manager
	-goto /media/pi/
	-the drive will be a folder
	-enter drive folder
	-copy folder path (with name) from file manager
	-should look like:
	  /media/pi/<driveName>
  note: You can enter a path to a folder on the local SD card if you want to exclude the flash drive 
        

Configure Program:

setup OWconfig.txt 
	
	A)set filePath value (auto-set by Installer)
		-open file manager
		-goto /media/pi/
		-the drive will be a folder
		-enter drive folder
		-copy folder path (with name) from file manager
		-should look like:
		  /media/pi/<driveName>
	B)set difference
		-amount of value change required for a pixel to
		 be marked as changed. Lower value will result 
		 in more more sensitive readings
	C)set pixels
		-number of pixels that need to be marked as 
		 changed in order to trigger security record
		 mode. 
	D)set width
		-The video image width resolution value.
		-default is set to 1280
		-max value 1920
	E)set height
		-The video image height resolution value.
		-default is set to 720
		-max value 1080
	F)set clipLen
		-The length of the video recordings taken when
		 security record mode is triggered. Given in
		 seconds.
	G)set MemThrshld
		-The max percent disk space used on usb drive 
		 before going into memory purge mode.


Run Overwatch by opening the desktop icon
	-Overwatch will open a small gui and a LxTerminal window
	-The gui has three buttons:
		Record: start overwatch recording sequence
		Open Video: browse for and play mp4 video
		Close Window: terminates gui
	-Upon running the recording sequence, the program will 
	 generate a folder tree on the usb drive. The folder tree
	 is called Videos, and it contains folders numbered 1 
	 through 24. Each folder will hold videos recorded during 
	 the associated hours. For example, videos recorded at 1am
	 appear in the 1 folder, and videos recorded at 1pm appear
	 in the 13 folder.
	-If the usb memory used exceeds the MemThrshld value then
	 the program will begin purging the video folders as it 
	 switches to that folder on the change of the hour. All
	 files inside the folder will be deleted when purged.
Method:
	The program uses the pi camera to detect motion by comparing 
	sample pictures over time. The samples are scanned pixel by pixel
	in greyscale to determine the change in shade value. If this 
	change is greater than the 'difference' value set in the OWconfig
	file, then the pixel is marked as changed. If the total number of
	changed pixels exceeds the 'pixels' value set in OWconfig, then 
	the program starts a new recording.
	When starting a new recording, the program sets the camera 
	resolution values to the 'height' and 'width' values set in the 
	OWconfig file. Also, the video's save path is set to the folder of
        the current hour of the system clock. While setting the path, the
	program scans the usb drive for percent memory used. If this 
	value exceeds the 'MemThrshld' value set in the OWconfig file,
	then the program will continue operation in purge mode. When
	program switches to a new hour's folder while in purge mode,
	the new hour folder will be cleared of all internal files as to 
	free up disk space. This will continue until the detected 
	percent memory used is less than the 'MemThrshld' value and 
	the program exits purge mode.
	Furthermore, the new recordings will have length in seconds 
	equal to the value of 'clipLen' in the OWconfig file. The videos
	are timestamped, encoded to MP4, and saved to the set folder. 
	After the recording is complete, the program will begin scanning 
	for motion again. 
Operation:
	After installing the system, begin use by opening the gui via the
	desktop icon. Click on the 'Record' button and system will be
	activated. It will continue to run until prompted to stop(Ctrl-C),
	loses connection with USB drive, or the system loses power. 
	
	To Review the videos, you can use the gui's built in video player,
	or you can simply remove the usb drive and view on a computer. 
	Removing the drive will halt the recording process, and upon 
	returning the drive, ensure it is plugged into the same port. 

	The gui's built in video player is based on raspbian's omxplayer.
	The player is VERY rough and offers NO VIDEO CONTROLS. Upon
	clicking the 'Video Player' button, a browser gui will allow you
	to select a video file. Upon selection, the program will start
	a video overlay on the main screen that will last the entire 	
	duration of the video clip. As of now there is no way to stop,
	pause, or seek the video once it is selected. After the video has
	finished, the overlay will be terminated.
	  


Baguda 08-2017
