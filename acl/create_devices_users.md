# How to create devices and users in my account?

You can create devices and users in two different ways:
- From the workspace, using the visual tools
- From your code, dynamically

## Create device/users from the workspace

Sign-in to your [workspace](https://www.scriptr.io/workspace), click on your username in the top-right corner of the screen, 
then  click on **Device Directory**

![Device Directory](./images/device_directory.png)

*Image 1*

- Click on +Add Device
- Enter values for the mandatory device id and password fields
- Optionally, you can optionally, you can assign the device to groups (the device will inherit from all the authorizations of the selected groups)
- Enter a optional description
- Click on the check sign

![Device Directory](./images/new_device.png)

*Image 2*

Scriptr.io automatically generates and authentication token for the newly created device. 
This token can be used to authenticate http requests and web socket messages sent to your APIs.