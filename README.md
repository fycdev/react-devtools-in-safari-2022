# React DevTools In Safari 2023

## Introduction
This is a successful experiment in getting React DevTools working in Safari Developer Tools in Safari 17.1.2 as of December 2023. Potentially, this process can be used with other extensions that extend the developer tools (e.g. Redux, Vue, etc...).

This is only a proof of concept but hopefully this may open the doors for tools that were previously accessible on other browsers to be now available on Safari spurring for a more open web experience (and debugging) for developers.

## Requirements
- MacOS 14+ (Sonoma)
- Safari 17.1.2
- XCode 15+
- Node 20+ (i.e., the latest LTS version supported by react, v20 at this time)

## Instructions

In summary, we will build Firefox's version of React DevTools, port that built version to a Safari extension then install.

1. Clone the [React repository](https://github.com/facebook/react) so we can build the Firefox React DevTools.

    ```
    git clone https://github.com/facebook/react.git
    ```

2. In the main `react/` directory, build the Firefox React DevTools.

    a. Download the build from their CI:

    ```
    cd scripts/release
    yarn install
    yarn add node-fetch@2
    ./download-experimental-build.js --commit main -r stable
    ```

    b. From the main `react/` directory, run the following:

    ```
    cd packages/react-devtools-extensions/
    yarn install
    yarn build:firefox
    ```

    This will output the built files (.zip folder and an unpacked folder) to `packages/react-devtools-extensions/firefox/build`.

    c. Unzip the zip file to a project folder of your choosing. Remember the folder path as it will be needed shortly.
    ```
    unzip /path/to/ReactDevTools.zip -d /path/to/dest/folder
    ```

3. Open Safari, in the Menu Bar enable the below.
    - `Develop -> Developer Settings... -> Allow Unsigned Permissions`
      (optionally you can opt to sign the extension instead).

4. Close Safari.
    
5. Convert the Firefox React DevTools to a Safari extension using `safari-web-extension-converter`.

    ```
    xcrun safari-web-extension-converter /path/to/dest/folder --macos-only
    ```

    Optionally, you can use the `--project-location` flag to set the destination folder for the converted extension otherwise it will save it to the current directory.

4. Xcode should automatically open the created project once the conversion has been completed. Otherwise, open the project in Xcode.
   
6. Build and install the extension by either pressing the "play" button or clicking `Product -> Run` from the Menu Bar. A window will open saying "Quit and Open Safari Extensions Preferences...", click the button below the text. Safari's extensions settings should now open.

7. If everything worked according to plan, React Developer Tools will be now shown in the extensions list.
   
   a. Enable it, then allow the extension to run on any chosen website or all websites.

   b. To uninstall, click the "Uninstall" button in the Extensions pane. Follow the prompt to open the folder. Navigate up the folders until `React_Developer_Tools-<somehash>`
   can be deleted.

9. Congratulations! You've now successfully installed React DevTools extension for Safari.

## References
[Converting a web extension for Safari](https://developer.apple.com/documentation/safariservices/safari_web_extensions/converting_a_web_extension_for_safari)

[Adding a web development tool to Safari Web Inspector](https://developer.apple.com/documentation/safariservices/safari_web_extensions/adding_a_web_development_tool_to_safari_web_inspector)
