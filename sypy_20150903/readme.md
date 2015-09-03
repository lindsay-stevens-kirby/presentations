# Sypy 20150903 Presentation

Can either read the markdown on its own (on Github for fancy styling), or get 
a copy of reveal.js and view it locally in a browser.

## Reveal.js Setup
This is how to do it for Windows, probably similar or easier on other systems.
- Get nodejs binary node.exe
- Get the latest node package manager (npm) github repository zip.
- Create the following folder structure:
```
C:/users/yourname/node
  - node.exe
  - node_modules/npm/bin (+ other directories from npm zip)
```
- Copy npm and npm.cmd from /npm/bin to /node.
- Make a new batch file at /node/setpath.bat containing this command:
```batch
  @ECHO OFF
  @SET PATH=%PATH%;C:/users/yourname/node
```
- Now to install http-server.
- Open a command prompt and enter the following commands.
```cmd
  call C:/users/yourname/node/setpath.bat
  npm install -g http-server
```
- Set up reveal.js.
- Make a new folder C:/users/yourname/presentations
- Get the latest reveal.js github source zip.
- Unzip reveal.js, and copy over the following folders:
  + C:/users/yourname/presentations
    - css/
    - js/
    - lib/
    - plugin/
- Put your (or my) presentation html files in the /presentations folder
- Now run the http-server so the presentations can be viewed.
- Open a command prompt and enter the following commands.
```cmd
  cd C:/users/yourname/presentations
  call C:/users/yourname/node/setpath.bat
  http-server
```
- Open a browser at localhost:8080/yourpresentation
  + It might be at 8081, check what http-server says when you started it.
- Enjoy!