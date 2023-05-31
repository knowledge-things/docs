# Setup VNC server on the Jetson developer kit 

1. Enable the VNC server to start each time you log in

   If you have a Jetson Nano 2GB Developer Kit (running LXDE)

   ```
   mkdir -p ~/.config/autostart
   cp /usr/share/applications/vino-server.desktop ~/.config/autostart/.
   ```

   For all other Jetson developer kits (running GNOME)

   ```
   cd /usr/lib/systemd/user/graphical-session.target.wants
   sudo ln -s ../vino-server.service ./.
   ```

2. Configure the VNC server

   ```
   gsettings set org.gnome.Vino prompt-enabled false
   gsettings set org.gnome.Vino require-encryption false
   ```

3. Set a password to access the VNC server

   ```
   # Replace thepassword with your desired password
   gsettings set org.gnome.Vino authentication-methods "['vnc']"
   gsettings set org.gnome.Vino vnc-password $(echo -n 'thepassword'|base64)
   ```

4. Reboot the system so that the settings take effect

   ```
   sudo reboot
   ```

   The VNC server is only available after you have logged in to Jetson locally. If you wish VNC to be available automatically, use the system settings application on your developer kit to enable automatic login.

5. 在Jetson设备上，VNC服务器只有在本地登录后才可用。如果希望VNC自动启动，可以使用系统设置应用程序来启用自动登录功能。以下是在Jetson设备上启用自动登录的一般步骤：

   1. 在Jetson设备上打开系统设置应用程序。
   2. 寻找和选择"用户"或"账户"选项。
   3. 在用户设置中，查找"自动登录"或"登录选项"。
   4. 启用自动登录选项，并选择要自动登录的用户。
   5. 保存更改并关闭系统设置应用程序。

## Connecting to VNC service from another computer 

You’ll need to know the IP address of your Jetson developer kit to connect from another computer. Run the ifconfig command on your developer kit and note down the IP address assigned to eth0 interface if using ethernet, wlan0 interface if using wireless, or l4tbr0 if using the USB device mode Ethernet connection.

[WINDOWS](https://developer.nvidia.com/embedded/learn/tutorials/vnc-setup#collapseOne)

Step 1: Download and Install VNC viewer from [here](https://www.realvnc.com/en/connect/download/viewer/)

Step 2: Launch the VNC viewer and type in the IP address of your developer kit

Step 3: If you have configured the VNC server for authentication, provide the VNC password

[MACOS](https://developer.nvidia.com/embedded/learn/tutorials/vnc-setup#collapseTwo)

The example below uses the Screen Sharing app included with macOS. However, any of your favourite vnc clients should work as well.

- Step 1. Open FInder and choose Go | Go to Folder from the menu bar.
- Step 2. Enter “/System/Library/CoreServices/Applications” and click Go
- Step 3. Open the app named Screen Sharing and enter the connection information. For example: username@
- Step 4. Click connect.
- Step 5. If you have configured the VNC server for authentication, provide the VNC password.

[LINUX](https://developer.nvidia.com/embedded/learn/tutorials/vnc-setup#collapseThree)

The example below is using gvncviewer, however any of your favourite vnc clients should work as well. One popular alternative is remmina.

- Step 1: Install gvncviewer by executing following commands:

  ```
  sudo apt update
  sudo apt install gvncviewer
  ```

- Step 2: Launch gvncviewer

  ```
  gvncviewer 
  ```

- Step 3: If you have configured the VNC server for authentication, provide the VNC password