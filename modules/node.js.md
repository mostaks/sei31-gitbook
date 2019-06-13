# Node.js

## Node Installfest <a id="node-installfest"></a>

* ​[nodejs.org](https://nodejs.org/en/)​

#### Open the node repl <a id="open-the-node-repl"></a>

`node`

#### check the version of node <a id="check-the-version-of-node"></a>

```text
node -v=> v9.4.1
```

#### The below line will either install or update node depending on if you have it installed <a id="the-below-line-will-either-install-or-update-node-depending-on-if-you-have-it-installed"></a>

`brew install nodejs`

#### Navigate back to the root directory <a id="navigate-back-to-the-root-directory"></a>

`cd ~`

#### Create a new folder <a id="create-a-new-folder"></a>

`mkdir .npm-packages`

#### Create a new file <a id="create-a-new-file"></a>

`touch .npmrc`

#### Open that file in ATOM <a id="open-that-file-in-atom"></a>

`atom .npmrc`

#### Paste the below line into file <a id="paste-the-below-line-into-file"></a>

`prefix=${HOME}/.npm-packages`

#### Go back to your terminal and open your `.bash_profile` file <a id="go-back-to-your-terminal-and-open-your-bash_profile-file"></a>

`atom ~/.bash_profile`

#### Add the below code to your `.bashrc` <a id="add-the-below-code-to-your-bashrc"></a>

`# Via https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md NPM_PACKAGES="${HOME}/.npm-packages" PATH="$NPM_PACKAGES/bin:$PATH"`

#### Run the below code to check if anything is inside the folder \(you may not have anything in there yet\) <a id="run-the-below-code-to-check-if-anything-is-inside-the-folder-you-may-not-have-anything-in-there-yet"></a>

`ls -l ~/.npm-packages`

#### Install learnyounode - globally <a id="install-learnyounode-globally"></a>

`npm install -g learnyounode` Learnyounode is an application that runs in the terminal to teach you how to use `node`

#### To open the application run <a id="to-open-the-application-run"></a>

`learnyounode`

