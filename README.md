
# R5 Setup Process

## Setup Vivado Environemnt
1. Generate bitstream for Vivado.
2. Export bitstream.
3. Save/Copy bitstream and .xsa files into accessible directory.

## Setup Vitis

### Install Vitis
1. Download Vitis Unified Installer from [https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html]
2. Install Vitis XX and XX with installer
- NOTE: You do not need to download any device packages as you will use the .xsa file from Vivado to define it.

### Patch Vitis
Once you have finished installing Vitis, you will need to patch it to avoid an error later on. Download the patch from the following link and follow its README. [https://support.xilinx.com/s/article/000034848?language=en_US].


### Server Port Thingy: so that we dont have to download 200GBs 
We didn't implement the following; however, we believe that this would enable students to setup a debug server on their computer connected to the board
and then remote into LRC and use the vitis debugger routed through their computer to the board.
This prevents people from having to install 200+ GBS of files Vitis requires Vivado
 
https://docs.xilinx.com/r/en-US/ug908-vivado-programming-debugging/Remote-Debugging-in-Vivado

### Create Platform Project
Before we can start programming the R5 we need to create a Platform. This is so that Vitis will know what hardware we are using. In order to do this, click File -> New -> Platform Project. Click Next to get to the first main page. Here you will want to click browse and select the .xsa file we saved from Vivado or you can use an example one from the RPU.


### Modify Board Support Package (BSP)
Edit the hardware platform RPU BSP to include LibMetal and OpenAMP and build the hardware platform


### Create Application Project
Now we need to create an application project. This will be the actual program we run. Create a Application Project File -> New -> Applicaiton Project. When creating make sure to select the previously created Platform Project.  On the Application Project Details page, make sure to select the R5 core in a new project system. Finally, choose one of the OpenAMP demos to start from or a different template.


### Create Debug Configuration??

### Build Hardware
Finally, you need to build the Hardware Release version. Click on the assistant Tab next to the file explorer. Then right click on the Hardware tab listed in you project system Tab and click build. Upload the produced elf file to /lib/firmware on your board.

## APU Setup
1. Upload the software from the APU folder to your ultra96 or modify for your project

## Linux Setup
1. Ensure Remote Proc Support
- Goto the APU diretory and either copy the example folder contents to your sdcard or use the provided dtsi file
and petalinux to generate a dtb that supports remoteproc


## Execute the Program

1. When in linux run modprobe zynqmp_r5_remoteproc
2. Load the fpga with the bit file from the PL directory
3. Next run: echo fpu_controller.elf > /sys/class/remoteproc/remoteproc0/firmware
- This loads the program into memory
4. Start RPU with: echo start > /sys/class/remoteproc/remoteproc0/state
5. Run the linux program you are going to use (You can only run it once)
6. Reset RPU with: echo stop > /sys/class/remoteproc/remoteproc0/state


## Additional Resources
1. https://docs.xilinx.com/r/en-US/ug1400-vitis-embedded/Debug-Perspective-in-Vitis-IDE
2. https://docs.xilinx.com/v/u/2021.1-English/ug1186-zynq-openamp-gsg
3. https://www.kernel.org/doc/html/latest/staging/remoteproc.html
