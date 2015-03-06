---
layout: post
title: "Xcode Bot Keychain Configuration"
date: 2015-03-05 15:56:00 -0700
comments: false
categories: 
---
The Xcode Bot server (as of 3.2.1) uses `/Library/Developer/XcodeServer/Keychains/Portal.keychain` to store the needed certs and keys used for codesigning (previously it used the `System.keychain`.

The server seems to only automatically download developement certs and profiles (not distribution) so we must manually populate the keychain with the dsitribution cert/key pairs needed and manually place the distribution profile(s) we need into the location which the server looks for them. In my [iOSContinuousIntegration](https://github.com/levigroker/iOSContinuousIntegration) system I have a `pre_action.sh` script which handles copying the profiles from where they are committed in the repository to the proper location on the server for the bots to locate them, so no need to go into that here. The keychain is a bit more complicated...

(Shamelessly borrowed from [http://stackoverflow.com/a/25934218/397210](http://stackoverflow.com/a/25934218/397210))

1. Copy the Portal keychain to your desktop:

        sudo cp /Library/Developer/XcodeServer/Keychains/Portal.keychain ~/Desktop/
        Password: your-administrator-password
        sudo chown `whoami`:staff ~/Desktop/Portal.keychain 

2. Set the Portal keychain password to "123"

        security set-keychain-password -o "`sudo cat /Library/Developer/XcodeServer/SharedSecrets/PortalKeychainSharedSecret`" ~/Desktop/Portal.keychain 
        New Password: 123
        Retype New Password: 123
    
3. Open the Keychain in Keychain Access:

        open -b com.apple.keychainaccess ~/Desktop/Portal.keychain

4. Unlock the "Portal" keychain using password "123" (This may not be needed as it may already be unlocked).

5. Add the needed keys to the "Portal" keychain. Generally you will want your AdHoc Distribution certificate and private key, assuming you're building for AdHoc distribution (for say, [Crashlytics Beta](http://try.crashlytics.com/beta/)).

6. Make sure the private keys have the correct access rights (in the "Access Control" tab of the "Get Info" for the private keys in Keychain Access), "xcsbuildd", "xcscontrol", "xcodebuild" and "codesign" should be listed. "xcsbuildd", "xcscontrol", and "xcodebuild" are inside the Xcode binary... `/Applications/Xcode.app/Contents/Developer/usr/bin` while "codesign" is `usr/bin/codesign`. Finder's "Go to Folder..." command is useful for locating these binaries. Though you will likely need to "unhide" the `/usr` directory from the GUI.

    This can be done by opening a terminal and issuing the command:
    
        sudo chflags nohidden /usr
        
    Then navigate to `/usr/bin` and select the codesign executable from the file picker GUI in the "Access Control List" tab of the Keychain Access's Get Info dialog. Once added to the ACL you can (and should) re-hide `/usr` with this command:
    
        sudo chflags hidden /usr

7. Lock the "Portal" keychain, quit "Keychain Access"

8. Reset the Portal keychain password:

        security set-keychain-password -p "`sudo cat /Library/Developer/XcodeServer/SharedSecrets/PortalKeychainSharedSecret`" ~/Desktop/Portal.keychain 
        Password: your-administrator-password (optional step)  
        Old Password: 123

    It may or may not ask you for your administrator password again, pay attention to the prompt.

9. Copy the Portal keychain back

          sudo chown _xcsbuildd:_xcs ~/Desktop/Portal.keychain
          sudo cp ~/Desktop/Portal.keychain /Library/Developer/XcodeServer/Keychains/
      
10. Since the system caches open keychains, restart you computer.

...all this because Xcode Bots don't copy non-development profiles.