# How To Update Node Versions (& Projects)

## Table of Contents

- [Purpose](#purpose)
- [Install New Node.js Version](#install-new-nodejs-version)
- [Update npm](#update-npm)
- [Install Global Packages](#install-global-packages)
- [Update Projects](#update-projects)
- [My Specific Process](#my-specific-process)
- [Regular Project Maintenance](#regular-project-maintenance)

## Purpose

This is intended as a guide to update Node.js versions, including making sure global packages are able to be used by the updated version, and that local projects are updated to work with the new version as well.

This guide uses nvm to manage node versions.

For an earlier version of this same guide, please visit [this link][1] to read the blog post.

## Install New Node.js Version

There are multiple commands that nvm allows you to use to install Node.js. `nvm install node` will install the latest Node.js version, while `nvm install 18.20.5` would install that specific version.

Once Node.js has installed, run `nvm use (insert version number)` to start using that version. You can also attach an alias name to that version with `nvm alias (insert alias name) (insert version number)`. So to set the alias of version 18.20.5 to "default", you would type `nvm alias default 18.20.5`.

For reference, my Node.js versions live in my nvm folder located at `/Users/dan/.nvm`.

To double-check that the new version of Node.js is running, use the command `nvm -v`. To see a list of all currently installed versions, use `nvm ls`.

## Update npm

If you didn't already know, installing Node.js will also install a specific version of npm paired with that Node.js version. For example, installing 18.20.5 will also install npm version 10.8.2. You can separately install a different npm version than the one that was installed with Node.js by running `npm install -g npm@latest` to install the latest npm version, or by replacing `latest` with a specific version number. This will install that npm version and replace the version that came with the version of Node.js that was installed in the `node_modules` folder for Node.js. Using the same Node.js version as an example, that folder should be found inside the nvm folder at `/.nvm/versions/node/v18.20.5/lib/node_modules/`.

## Install Global Packages

Globally installed packages that you may have used with a previous Node.js version do not automatically port over to the new version. They need to be installed into the same `node_modules` folder as in the previous example of installing a version of npm.

If you wanted to install `create-react-app`, for example, you would run `npm install -g create-react-app`.

Another method to more easily install all global packages to use with the new Node.js version is to run `nvm install node --reinstall-packages-from=node`. This installs the latest version of Node.js and also installs the global packages you were already using.

You can also specify which versions to install and which version to take global packages from. `nvm install 18.20.5 --reinstall-packages-from=18.20.4` will install Node.js version 18.20.5, and into that version install the global packages currently installed in Node.js version 18.20.4.

And one more tack-on can also install the latest npm version, not the one that is linked to the Node.js version by default. `nvm install node --reinstall-packages-from=node --latest-npm` would install the newest Node.js version, bring all your global packages over from the currently-running Node.js version, and install the newest npm version as well.

_Note: I did run into an issue where running the command `nvm install node --reinstall-packages-from=node` installed global packages from the latest Node.js version installed on my system, not the Node.js version I was currently running. So for example, I was running Node.js version 18.x.x and was updating to a newer 18.x.x but also had version 20.x.x installed. Running the above command installed global packages from the 20.x.x version and not the 18.x.x version that was being used at the time._

## Update Projects

You should be able to simply run `npm rebuild` in the directory of your projects that includes their `/node_modules/` folders to update each project to work with the newly installed version of Node.js.

## My Specific Process

Install desired Node.js version (typicall latest minor version of whichever I'm currently using - x.X.x), including global packages from the currently-used version, and the latest version of npm

`nvm install x.x.x --reinstall-packages-from=x.x.x --latest-npm`

Check that everything installed correctly in the `/Users/dan/.nvm` folder. Then check to see which npm version installed

`node -v`

Set newly installed Node.js version to be the one running

`nvm use x.x.x`

And set it to the alias "default"

`nvm alias default x.x.x`

Make sure the correct version is currently running

`node -v`

Update all projects to use the new Node.js version. In the root project directory where the `/node_modules/ directory lives, simply run

`npm rebuild`

Then check to make sure the project can run in development mode

Remove the previously used Node.js version if desired

`nvm uninstall x.x.x`

## Regular Project Maintenance

Current projects in maintenance mode:

- Buda Fooda
- Portfolio
- Simple Gym
- Gulp Framework

For each project, run `npm audit` and `npm audit fix` to fix any security issues that can be fixed with this method.

Run `npm outdated` to see all outdated dependencies and whether they can be updated with a major or minor version. `npm update --save` will update any dependencies to the newest minor version and note those changes in `package.json`. Then decide to update to any newer major versions after checking docs for breaking changes.

Update package.json files to note new minimum Node.js version

Update any relevant README files and package.json to change project version numbers and what was changed. Version numbers can be increased by 0.01 unless dependencies had major updates.

Test updated projects in development mode before committing then make sure the deploys work.

[1]: https://codepen.io/danbuda/post/how-to-update-version-of-nodejs
