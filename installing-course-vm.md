---
layout: default
title: Course-VM
nav: course-vm
---

While you are welcome to install a C++ compiler or integrated development environment natively on your system, or work remotely on aludra.usc.edu (which we consider rather inconvenient), we strongly encourage you to use the <a href="http://www.ubuntu.com">Ubuntu</a> virtual machine (VM) specifically provided for this class. The VM that we use is a customized Ubuntu LTS installation that comes with the most recent C++ compiler, libraries, and debuggers. You can install it on your laptop regardless of your operating system, and use it for the entire semester for labs and homework assignments. We will grade everything on this VM's compiler version and environment so it is critical you check your code on this system before submitting. <strong>All C++ compilers are <span style="text-decoration: underline">NOT</span> the same.</strong> The code you write on Visual Studio or XCode (common Windows and Mac development environments) may not run the same way on another system.

<h3>Installation instructions</h3>
<ol>
	<li>To run this virtual machine you will need to download <a href="https://www.virtualbox.org/wiki/Downloads">Oracle VirtualBox</a>. Download version 5.0</li>
	<li>After installing VirtualBox, download and install the extension pack, available on <a href="https://www.virtualbox.org/wiki/Downloads">the same downloads page</a>.</li>
	<li>Next download the virtual machine image. We recommend using 'curl' which is already installed on Mac and Linux machines. (A Windows version is available <a href="http://www.confusedbycode.com/curl/">here</a>.) 'curl' is a command line utility to download files from the Internet. Go to a folder where you want to download the file and start a command prompt (Windows) or Terminal there. Then run the command
<pre>curl http://bits.usc.edu/files/cs103/install/student-vm-2015.ova -o student-vm-2015.ova</pre>
<ul>
	<li>Alternatively, the actual link is available <a href="http://bits.usc.edu/files/cs103/install/student-vm-2015.ova">here</a>. Using curl is recommended because browser downloads might disconnect unexpectedly.</li>
	<li><b>[Optional] You can also try using an FTP client, which will support resuming stopped downloads. For example you can download the FileZilla FTP client from
https://filezilla-project.org/download.php?type=client
Once installed, connect to host "bits.usc.edu" and inside the "cs103" folder, inside its "install" subfolder, download the ova file.</li>
</ul>
</li>
	<li>Start Virtual Box and choose File...Import. Then select the Ubuntu Virtual Machine (student-vm-2015.ova) you downloaded. Use the default import options.</li>
	<li>Adjust the settings of your VM
<ul>
	<li>Adjust the appropriate amount of base memory. Everything has to be in the green zone:
<img class="alignnone size-medium wp-image-1551" src="http://bits.usc.edu/cs104/wp-content/uploads/sites/12/2014/12/mainboard-300x139.png" alt="mainboard" width="300" height="139" /></li>
	<li>Make sure the VM has at least 2 CPU cores allocated. You can adjust this later to get the best results.
<img class="alignnone size-medium wp-image-1550" src="http://bits.usc.edu/cs104/wp-content/uploads/sites/12/2014/12/cpu-300x110.png" alt="cpu" width="300" height="110" /></li>
	<li>Turn 3D Acceleration ON.
<img class="alignnone size-medium wp-image-1549" src="http://bits.usc.edu/cs104/wp-content/uploads/sites/12/2014/12/3d-300x110.png" alt="3d" width="300" height="110" /></li>
	<li>Now lick on the Course VM option that now appears in VirtualBox's list and select Start/Run. This will start the VM and bring you to a logon message. (Answer yes or default answer to any dialog box that appears).</li>
</ul>
</li>
	<li>If you encounter errors starting your VM go to the <a href="#Troubleshooting">Troubleshooting Section</a> and then resume these directions.</li>
	<li>Finishing the setup
<ul>
	<li>Login with the credentials:
<strong>username</strong>: <tt>student</tt>
<strong>password</strong>: <tt>developer</tt></li>
	<li>Hit Ctrl-Alt-T to start a terminal window where you can type in commands</li>
	<li>Install the Virtual Box Guest Additions as detailed in the <a href="#DosDonts">Do's and Dont's</a> Section</li>
	<li>Pick and setup a method to backup your files as detailed in the <a href="#DosDonts">Do's and Dont's</a> Section</li>
	<li>If you haven't worked with Linux, check out a Linux tutorial such as <a href="http://www.ee.surrey.ac.uk/Teaching/Unix/">this one</a> (Tutorials 1-6) or possibly <a href="http://vic.gedris.org/Manual-ShellIntro/1.2/ShellIntro.pdf">this one</a>.</li>
	<li>For starters, work through this <a href="http://cplusplus.com/doc/tutorial/">tutorial</a>. Start from the beginning and continuing through pointers. Write down any questions or unclear statements. We can discuss them at the beginning of the semester. Also, I have made lecture videos on most of these topics available at this <a href="http://ee.usc.edu/~redekopp/csmodules.html">CS Modules Site</a>. Please be sure you know the material covered in the first 3 modules (C++ Introduction and Control Structure and Functions) before coming to class. See the next section for details</li>
</ul>
</li>
</ol>
<h3 id="toc_1a"><a name="DosDonts"></a>Do’s and Don’ts</h3>
<ul>
	<li>When you shutdown your VM <b>NEVER</b> "Save the State" of the machine but instead "Power off" the machine or send the "Shutdown Signal"</li>
	<li>In your Linux VM do <b><span style="text-decoration: underline">NOT</span></b> install any updates or upgrades to the OS or other source if it prompts you. Just use the VM as it is.</li>
	<li><b><span style="text-decoration: underline">DO</span></b> install the “Guest Additions” to your Linux VM. This will allow you resize the resolution/window and also support shared folders between your Host and Guest OSs. To do this, start your VM and click the Devices Menu..Install Guest Additions. You may have to enter your password (“developer”) or hit ‘Enter’ once or twice, but other than that it will just run and take a few minutes. When complete it will say “Hit Enter to close the window”. At this point restart your VM and everything should be working.</li>
	<li><b><span style="text-decoration: underline">DO</span></b> find a way to back up your code on the VM. This is not as important, because you will learn how to use git, a version control system, to maintain and save your code. That will automatically act as a reliable backup option, if used correctly. However, here are alternatives:
<ol>
</li>
	<li><strong>Shared Folders. </strong>You can use the shared folders feature that is part of the VirtualBox service. Follow these steps to create a shared folder in VirtualBox. (note: “guest” or “VM” means the Linux box that you run your code on, while “host” means the system that you normally run.)
<ul>
	<li>Make sure that "Guest Additions" is installed.</li>
	<li>Make a folder called “cs104_files” somewhere on your host machine.</li>
	<li>Open VirtualBox and open the settings for the VM.</li>
	<li>Click on the “shared folders” button.</li>
	<li>Click the somewhat-obscure folder with a green + on it to add a shared folder to the list choosing . In the Folder Path box browse to the folder on the host that you want to share/make available (i.e. the “cs104_files” you just created). The Folder Name will automatically populate with the folder you just chose. Make sure “Auto-mount” is checked.</li>
	<li>Press OK.</li>
	<li>Open up the VM.</li>
	<li>Open a Terminal window.</li>
	<li>Type <code>sudo usermod -a -G vboxsf student</code>. This gives student the permission to access shared folders.</li>
	<li>Type <code>ln -s /media/sf_cs104_files cs104_files</code>. This creates a new alias in the current folder to where the shared folder was mounted.</li>
	<li>From here you can treat cs104_files just like any other folder on your linux guest.</li>
</ul>
</li>
</ol>
</li>
</ul>
<h3 id="toc_2"><a name="Troubleshooting"></a>Troubleshooting</h3>
In this section, we briefly go over common problems with VirtualBox and Ubuntu.
<ul>
	<li>If you're VM can't connect to the Internet but your host PC can, just restart the VM.  It is common for the VM to have trouble connecting if you leave it open when you put your laptop to sleep, etc.  Simply rebooting the VM usually reconnects to the network.
	<li>In the "Settings" menu, if there is a sign at the bottom of the window that reads "non-optimal", it means you have chosen a wrong setting. Hover your mouse over the warning message to get the details.</li>
	<li>The error “Failed to install NtCreateSection monitor” on Windows can be due to a known bug. Try downloading the <a href="https://forums.virtualbox.org/viewtopic.php?f=6&amp;t=62615">test build here</a>.</li>
	<li>Error “VT-x features locked or unavailable in MSR”: You need to enable Virtualization for your laptop. If you don’t do this, Ubuntu won’t be able to take advantage of all your CPU power. Usually virtualization is disabled by default on PC laptops and enabled by default on Mac laptops. Here is how to enable it on Windows:
<ol>
	<li>Enter the bios settings. This is different from laptop to laptop so you have to Google it and find the instruction for your make and model. For example something like this “Laptop HP dv6 bios virtualization”. Usually, you have to keep pressing F2, F10, or something similar at the very beginning of your laptop power on. This is before Windows starts.</li>
	<li>Find the “Virtualization” setting in the sub menus and set it to “ON” or “Enable”.
<img title="Enable Virtualization in Bios" src="http://www-scf.usc.edu/~csci104/20142/installation/bios.png" alt="Enable Virtualization in Bios" /></li>
	<li>Save and Exit.</li>
	<li>Older laptops might not have a virtualization option. In that case switch back to single-core VM.</li>
	<li>If problems still persist, try uninstalling VirtualBox 4.3.18 in favor of an older version 4.3.12 or 4.3.14 (start with 4.3.12) available at the <a href="https://www.virtualbox.org/wiki/Download_Old_Builds_4_3">Older Build Site</a>. Once you've uninstalled 4.3.18 and reinstalled 4.3.12 or 14, then re-import the VM image (course-vm-2014.ova)</li>
	<li>If you can't connect to the Internet from your VM, simply try rebooting the VM (not your whole PC). When the wireless connection changes the VM seems to be unable to pick up the new connection w/o a reboot.</li>
</ol>
</li>
</ul>