# LocalChat
LocalChat is a AI chatbot that can anaylse, summarise, and answer questions given text-based documents as input.

The app differentiates itself from other LLM services by running entirely locally within the user's web-browser.
The app does make any requests over the local-network or the internet - ensuring that your data exists only 
within your local device.

Additionally, the app does not require any software dependencies to run. Simply download and run.

## Running the Application

The app runs simply via double-clicking the `index.html` file included within the app distribution. This opens the file
using the `file:///` protocol, requiring only the browser to run the app. Meaning the app will be sand-boxed within the 
browser's execution environment.

The app is currently in the early stages of development and does not have a readily available distribution.

Currently, to run the app one must build the distribution from source. 

### Building from Source

Prerequisites:
- Install the Go toolchain - https://go.dev/doc/install
- Install nvm (node & npm) - [nvm for linux](https://github.com/nvm-sh/nvm) or [nvm for windows](https://github.com/coreybutler/nvm-windows)

Clone the repo to your local device with `git clone https://github.com/andrewtran3643/LocalChat.git`.

Cd into the web_app folder, `cd LocalChat/web_app`.

Run `npm install` to install the npm dependencies.

Run `npm run build-local-chat {target-operating-system}` to build the distribution (where `target-operating-system` is 
either `linux` or `windows`). This distribution should be found in the root directory of the project, `LocalChat/dist`.

> Optionally, run `npm run build-local-chat {linux|windows} wm` to build the distribution without downloading the models.

If using chrome or edge browsers simply double-click the `LocalChat/dist/index.html` file to run the app (this should
open the app in the browser using the file protocol).

If using Firefox, the app must be ran with a local fileserver executable, on Windows `LocalChat/dist/fileserver.exe`,
on Linux `LocalChat/dist/fileserver`. Open http://localhost:8080/ in your browser and the app should be running.
