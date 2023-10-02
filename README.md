# Findmydevice Wiki 
This is an in progress wiki for findmydevice.

[1. 📱 Introduction](https://github.com/ColoursofOSINT/findmydevice#1--introduction)

[2. 💬 Notifications (SMS support) vs 🌐 Server](https://github.com/ColoursofOSINT/findmydevice#2--notifications-sms-support-vs--server)

[2.1 💬 Notifications (SMS support)](https://github.com/ColoursofOSINT/findmydevice#21--notifications-sms-support)

[2.2 🌐 Server](https://github.com/ColoursofOSINT/findmydevice#22--server)

[3. 🖥 Commands](https://github.com/ColoursofOSINT/findmydevice#3--commands)

[4. ⛓️ Extra Permissions](https://github.com/ColoursofOSINT/findmydevice#4-%EF%B8%8F-extra-permissions)

[5. ❌ Known Issues](https://github.com/ColoursofOSINT/findmydevice#5--known-issues)

## 1. 📱 Introduction 
FindMyDevice is an Android App that severs as a Free and Open Source alternative to Google’s FindMyDevice. It allows you track, wipe and issue other commands to your device when it's lost. 

## 2. 💬 SMS vs ⌨️ Messages vs 🌐 Server 
FindMyDevice allows for commands and information to be sent and received via SMS, messaging apps and/or a server, depending on the configuration that has been set up.

### 2.1 💬 SMS
Text messages can be configured with a pin or known contact number. With a pin, any number can issue commands and receive information, assuming the pin is correct. With contact whitelisting, any chosen numbers can communicate with FindMyDevice. Both a pin and contact whitelisting can be configured simultaneously. In order for SMS to work, permissions to view SMS messages is required.  To view the full list of commands, go to the commands section of this wiki and see those marked with the SMS signal (💬). 

A pin allows you to communicate with fmd with a non-whitelisted contact. If you activate this setting and set a pin you can communicate using a phonenumber that is not added to the whitelist. Just text `fmd (pin)` and the sender's phone number is put on a temporary whitelist. The session expires after 10 minutes and than no communication is possible anymore. If you need more time, you can reactivate the number with the pin again.

### 2.2 ⌨️ Messages
With access to notifications, FindMyDevice can handle messages from other applications. Some applications will not work, but supported messaging apps include Telegram and Whatsapp.

### 2.3 🌐 Server
A server can be configured to use a server to issue commands and receive information. When enabled your phone constantly sends end-to-end encrypted your location to the server where you can than access it from a webbrowser. Your data is encrypted on the phone (with a generated private key) and than sent to the server. The server also has the privat key in an encrypted state that will be send to your webbrowser. The webbrowser than decrypts the key with your password and afterwards the data is decrypted and shown to you.

You can run use the defaut server [here](https://fmd.nulide.de:1008/), or you can a custom sever either writen in [Go](https://gitlab.com/Nulide/findmydeviceserver) (the offical implementation) or [PHP](https://gitlab.com/Playit3110/FindMyDeviceServerPHP). 

When you register you need to enter a password, this password will be needed on the webpage to decrypt the locationdata. When you are registered, everytime your phone gets located the location will also be transmitted to the FMDServer. After you have registered your phone, please note your custom User ID somewhere safe. You need this User ID to locate your phone. The userid is a generated random ID from the server so the data can be identified as anonymously. 

One can also enable FMDServer (the checkbox at the top) that allows the application to constantly send the location to FMDServer (every custom period).

In order to receive a command from the server (locate/ring/etc...) unified push is used, therefore you need to install a distributor (like Gotify-UP or ntfy). When you send a command like ring, the server will communicate with your push-server. The push-sever will notify your phone.
If you want to, you can register for my push-server (gotify-instance) (https://push.nulide.de:9094/#/login).

## 3. 🖥 Commands

You can set a custom prefix (default is fmd). Commands are:

```
fmd locate - sends the current location
fmd ring - lets the phone ring
fmd lock - locks the phone
fmd stats - sends device informtions
fmd delete - resets the phone
fmd camera (back/front) - takes a picture and sends it to the server
```

#### FMD Locate
`FMD locate` will send you the coordinates of your phone. In order to work, the phone needs Location enabled or this [Permission](PERMISSION WRITE_SECURE_SETTINGS) enabled. The application send the GSM_Informations (MCC, MNC, LAC, CID). These are the information about the cellular_tower you are connected with. You can get the location of this tower by searching on http://www.cell2gps.com/ or if you have added your [OpenCellID_API_KEY](OpenCellID_API_KEY) you can even configure FMD to send you the coordinates. The GPS-Coordinates will be send as soon as they are available to the device.

`FMD locate last` will send you the last known location

`FMD locate gps` will only send you the gps coordinates

`FMD locate cell` will only send you the cell location.

#### FMD Ring
`fmd ring` lets the phone ring with you default ringtone.

The phone will ring for 15 seconds. If you send `fmd ring long` it will ring for 3 minutes.

#### FMD Lock
`fmd lock` locks the device and displays a message that you can configure in the settings. If you lost you device and didn't configured a message you can just write `fmd lock (your message)` and it will be shown. After locking your phone a splashscreen with your preconfigured or send via sms message will be shown. You are now locked out of your phone and are able to move to the lockscreen of android via the android controls     `○   <`. Now you can enter your pin, password, fingerprint or other locking method and unlock your phone.

#### FMD stats 

#### FMD Delete
`fmd delete` is capable of deleting you phone. For this you have to configure a pin in the settings and enable this option. When you than send the command you have to add the pin at the end. 
For example:

`fmd delete <your pin>`

#### FMD Camera
`fmd camera` takes a picture from the camera you selcted(Back/Front) and sends it encrypted to the server. When you log in to the server you will be able to view the picture by presisng the picture icon.


## 4. Settings
### FMD delete
Here you can enable or disable the `fmd delete` command.
You enable this setting, when you activated the device admin, but don't want to remotely reset your phone.
If this option is disabled the device can't be reseted via fmd.

### Pin
Here you can enter a Pin. The pin is useful for "fmd delete" and for "fmd via pin". If no pin is entered "fmd delete" and "fmd via pin" will not work.

### fmd lock and Lock Screen Message
Here you can write a message that can be displayed on the lockscreen when you send the command 'fmd lock'.

### fmd command
Here you can change the default command for communicating with fmd. If you change the command you need to use your custom command for communication.

### OpenCellID
### API-Key
Here you can add the API-Key from OpenCellID. With their service you can get the coordinates of the celltower you are conected with. Please look at [API-Key](OpenCellID_API_KEY) for instructions.

### Permissions
Add some features by enabling their permission, you can do this by pressing the button, which will open the Permission-Wizard.

## 5. ⛓️ Extra Permissions
## 6. ❌ Known Issues


FMD Server allows to store the last locations of you phone.

When enabled your phone constantly sends e2e encrypted your location to the server where you can than access it from a webbrowser.

The default server is [https://fmd.nulide.de:1008/](https://fmd.nulide.de:1008)

Your data is encrypted on the phone(with a generated private key) and than send to the server. The server also has the privated key in an encrypted state that will be send to your webbrowser. The webbrowser than decrypts the key with your password and afterwards the Data.

#### How to use FMD Server

In order to use the server you need to enter the settings of fmd on your phone. Register on the default server or change the url and register on a custom server.

When you register you need to enter a password, this password will be needed on the webpage to decrypt the locationdata.

When you are registered, everytime your phone gets located the location will also be transmitted to fmdserver.

After you have registered your phone, please note your custom userid somewhere safe. You need this id to locate your phone. The userid is a generated random id from the server so the data can be identified as yours.
(Don't worry as long nobody knows whos id it is, nobody nows what data is yours and also it is encrypted)

You can also enable fmdserver(the checkbox at the top) that allows the application to constantly send the location to fmdserver.

#### How to get the location

Visit the default server [https://fmd.nulide.de:1008/](https://fmd.nulide.de:1008) or your custom one.

Enter the id of your device. You will be asked for the password, after you have entered it you can see the last known location and also can navigate through a history of locations.

### Push

In order to receive a command from the server (locate/ring/etc...) unified push is used.
In order to use it you need to install a distributor (like Gotify-**UP** or ntfy).
When you send a command like ring. The server will communicate with your push-server. The push-sever will notify your phone.
If you want to, you can register for my push-server(gotify-instance) (https://push.nulide.de:9094/#/login)

6. `fmd stats` shows you the networks available to the lost device.
This is helpful when you get desperate, maybe you recognize a network and can find the device than.

7. Hello, this is the wiki for FMD.

Here you will find some explanations how FMD works and how to configure it by your needs.

## Syntax

To start:

FMD is an application that can search for your device when it gets lost.

In order to find your device you need to have installed the application onto the device and add some of your contacts to your whitelist. When your device gets lost, just let one of your whitelist-contacts write a sms to your phone that starts with **fmd**.

Than the Syntax will be send to you. This may look like this:

```plaintext
FindMyDevice Commands:
fmd locate - sends the current location
fmd ring - lets the phone ring
fmd lock - locks the  phone
fmd stats - sends device informtions
fmd delete - resets the phone
fmd camera (back/front) - takes a picture and sends it to the server
```

You can send these commands as sms to the device so the action will be executed. If you want to see what the commands do you just select one:

- [FMD locate](FMD%20locate)
- [FMD ring](FMD%20ring)
- [FMD lock](FMD%20lock)
- [FMD stats](FMD%20stats)
- [FMD delete](FMD%20delete)
- [FMD camera](FMD%20camera)

The Settings that you can tweak in FMD are explained on the following page: [Settings](Settings)

## Interesting

* [ThirdParty Support](ThirdParty)
* [Push Support (UnifiedPush/ntfy)](PushSupport)

8. OpenCellID is a collection of celltower and there location.
You can register on their [page](https://opencellid.org/) and retrieve an API_KEY.
After you've created an account go to Access Tokens and copy the token. (Something like: pk.xxxxx)

Please enter this key to the settings of the application. When you than try to get the location the coordinates of the celltower will be send too.

9. For FMD to be able to turn Location Services on or off the special [`WRITE_SECURE_SETTINGS` permission](https://developer.android.com/reference/android/Manifest.permission#WRITE_SECURE_SETTINGS) is needed.

## Option 1: Grant from computer

To grant the permission from a computer via adb (Android Debug Bridge):

1. Install `adb` on your computer (e.g. `sudo apt-get install android-tools-adb`)
2. Enable Developer Settings on you device. Go to "Settings -> About Phone" and than click several times on "Build Number". You will be asked to enter your password if you have one.
3. Go to "System -> Developer Options" and enable "USB debugging"
4. Plug your phone into the computer
5. Open a terminal/cmd on your computer
6. Check that your device shows up by entering this command: `adb devices`
7. Grant the permission by entering this command:

`adb shell pm grant de.nulide.findmydevice android.permission.WRITE_SECURE_SETTINGS`

If adb returns an error like in [#15](https://gitlab.com/Nulide/findmydevice/-/issues/15)
you can try the following:

1. Go to Developer Settings
2. Enable USB-Debugging
3. Enable Install via USB
4. Enable USB-Debugging (Security Settings)
5. Rerun `adb shell pm grant de.nulide.findmydevice android.permission.WRITE_SECURE_SETTINGS`

## Option 2: Grant on device (requires root)

If you have a rooted device, you can use any terminal app to grant the permission:

1. Open your terminal app of choice
2. Run `su -c 'pm grant de.nulide.findmydevice android.permission.WRITE_SECURE_SETTINGS'`

## Check that it worked

To check that it worked:

1. Close FMD
2. Swipe it away from Recents
3. Reopen FMD

Under "Permissions" the entry "Secure Settings" should now be green.

10.n FMD uses [UnifiedPush](https://unifiedpush.org/) to receive push notifications.
Hence you need to have a [distributor app](https://unifiedpush.org/users/distributors/) installed on your phone if you want to use push.
Then FMD Server will act as the *application server*, and e.g. ntfy Server will act as the *push server*.

The recommended one is [ntfy](https://ntfy.sh/), but other UnifiedPush distributor apps should work, too.

After installing a push distributor app, remove FMD from the "Recent Apps" switcher by swiping it away, and then reopen it. This forces FMD to retry registering itself with the distributor.

No sensitive data is sent in push notifications, they are only used to wake up FMD.

## Using a private ntfy server

1. Configure the ntfy server in the **ntfy app** under Settings -> Default Server
2. Make sure that you have enabled anonymous write access to `up*` topics: https://docs.ntfy.sh/config/#example-unifiedpush

13.Third Party apps that show notification can be used in fmd.

If you have enabled the Notificationoption in fmd this happens automatically.

Since the whitelist cannot be used for identification the pin is needed and has to be set.

Please send the pin with every message right after fmd.

So the command for locate will look like this if communicating via a third party app.

`fmd topsecretpin locate`

14. The problem in this versions was, that for authentication i just could let the user try to decrypt the private-key. If he succeeds he would have access to the data, cause he an decrypt the data with the private-key. The private-key is encrypted with aes-256. So it's technically impossible somebody could brute-force it.

The problem is that even when the user does not decrypt the private-key he could download all the data from the server, cause i didn't add an authenticator like a token or something similar (my fault). As long as he manages to find out an device-id. So that's the first problem. I wasn't able to guarantee that only the user has access to the data.

The second one is, that due to the fact, that there is no token or something similar, every device could send data to the server and saves them. That means an attacker could send a data package to the server and it will be saved to folder with the id he transfers. As mentioned earlier, as long as the attacker finds out one id he can download the data(but can't view it) and upload data(that's critical).


To fix this a re registration is needed. (also to guarantee that no malicious data is on the server.
Now when somebody registers, the device sends besides the encrypted private-key a hash of the password and the publickey.
The publickey will be needed for the furture(more secure authentifiation)
When somebody wants to access the data. he has to send a hash of the passowrd and the id of the device he wants to get access to, than the server checks if the data matches and grants access to the data. With the password he can than decrypt the data that server will send.
Still the data is end to end encrypted.

15. Here is a list of solutions that may help you when you are stuck.

### Using multiple users on the same device.

If you are using multiple users on the same device, fmd may not work.
To solve this install fmd on all users and allow them to use sms.
You can do so in the user-settings.
Also if you don't want to receive multiple messages change the fmd_command for every user to a different one.

### Using Signal

This is not really a workaround but may happen to some of you.
When a device wants to communicate with fmd via the messaging app signal, please note that you need to send the message as sms.
Therefore you need to hold the blue sending button and select "Insecure SMS".

### RCS

If you try to message FMD via RCS (Rich Communications System), FMD will not respond. Right now only standard sms are supported, so you need to turn it off at least when you want to message fmd.

### Multi-SIM

If you have more than one SIM-card inserted, please select the default sim for sms-messaging in your settings app, so fmd can automatically send data via SMS.
