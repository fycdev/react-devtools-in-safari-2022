# React DevTools In Safari 2022

## Introduction
This is a successful experiment in getting React DevTools working in Safari Developer Tools in Safari 16 as of September 2022. Potentially, this process can be used with other extensions that extend the developer tools (e.g. Redux, Vue, etc...).

This is only a proof of concept but hopefully this may open the doors for tools that were previously accessible on other browsers to be now available on Safari spurring for a more open web experience (and debugging) for developers.

## Requirements
- MacOS 12+ (Monterey)
- Safari 16 (as of this date this would be Safari Technology Preview for your respective MacOS version)
- XCode 13+
- Node 14+

## Instructions

In summary, we will build Firefox's version of React DevTools, port that built version to Safari, make a few adjustments, and then install it in Safari Technology Preview.

1. Clone the [React repository](https://github.com/facebook/react) so we can build the Firefox React DevTools.

    ```
    git clone https://github.com/facebook/react.git
    ```

2. In the React directory, build the Firefox React DevTools

    a. Download the build from their CI (alternatively, you can build from source).

    ```
    cd scripts/release
    yarn install
    ./download-experimental-build.js --commit main -r stable
    ```

    No need to worry where the download is stored.

    b. From the main `react/` directory

    ```
    cd packages/react-devtools-extensions/
    yarn build:firefox
    ```

    This will output the built files (.zip folder and an unpacked folder) to `packages/react-devtools-extensions/firefox/build`.

    c. Unzip the zip file to a project folder of your choosing. Remember the folder path as it will be needed shortly.
    ```
    unzip /path/to/ReactDevTools.zip -d /path/to/dest/folder
    ```

3. Convert the Firefox React DevTools using `safari-web-extension-converter`

    ```
    xcrun safari-web-extension-converter /path/to/built/extension --macos-only
    ```

    Optionally, can use the `--project-location` flag to set the destination folder for the converted extension otherwise it will save it to the current directory.

4. `manifest.json` will need to be updated before the project is run. Xcode should automatically open the project once the conversion has been completed. Otherwise, open the project in Xcode.

    a. Within the project, navigate to `manifest.json` (case sensitive). If none of the file names have been changed it should be under `/React Developer Tools Extension/Resources/manifest.json`.

    b. Within the `permissions` property add `"devtools"` to the array.

    ```
    "permissions": [
        "devtools",
        "file:///*",
        "http://*/*",
        "https://*/*",
        "clipboardWrite"
    ]
    ```

5. Run the project by either pressing the "play" button or from the Menu Bar `Product -> Run` which will install the extension as an application. An app will open and will have a button to "Quit and Open Safari Extensions Preferences...". Ignore it and close the app.

6. Open Safari Technology Preview (since of this writing it is the only version with Safari 16).

    a. In the Menu Bar enable:
    - `Develop -> Allow Unsigned Permissions`
    - `Develop -> Experimental Features -> Web Inspector Extensions`

    b. Open Preferences and navigate to Extensions. React Developer Tools will be shown in the list given the process worked successfully. Enable it and allow the extension to run on any chosen website or all websites.

    c. To uninstall, click the "Uninstall" button in the Extensions pane. Follow the prompt to open the folder. Navigate up the folders until `React_Developer_Tools-somehash` can be deleted.

7. Congratulations! React DevTools should now be working in Safari.

## References
[Converting a web extension for Safari](https://developer.apple.com/documentation/safariservices/safari_web_extensions/converting_a_web_extension_for_safari)

[Adding a web development tool to Safari Web Inspector](https://developer.apple.com/documentation/safariservices/safari_web_extensions/adding_a_web_development_tool_to_safari_web_inspector)