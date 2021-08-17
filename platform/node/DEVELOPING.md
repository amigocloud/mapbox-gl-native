# Developing the Mapbox GL Native Node.js module

This document explains how to build the [Node.js](https://nodejs.org/) bindings for [Mapbox GL Native](../../README.md) for contributing to the development of the bindings themselves. If you just want to use the module, you can simply install it via `npm`; see [README.md](README.md) for installation and usage instructions.

We use [CMake:3.18.3](https://cmake.org/cmake/help/latest/) to build Mapbox GL Native for various platforms, including Linux, Android, iOS, macOS and Windows. 

The following dependencies are platform-specific to the OS you are using to build the package from source

**Linux:**

- xvfb
- mesa-utils
- libosmesa6-dev
- libgl1-mesa-glx
- libgl1-mesa-dev
- libegl1-mesa-dev
- libglfw3
- libglfw3-dev
- mesa-common-dev
- libuv1
- libuv1-dev

**MacOS:**

- glfw3
- gcc@7


Then using Node 10.x run the following commands from the root of this repository tree. It will build Mapbox GL Native targeting your host architecture given that you have all the dependencies installed.

```bash
# Update the vendors submodules
git submodule update --init --recursive    

# Install the package dependencies
npm install

# Remove the pre-compiled binary from the official repo
rm -rf lib

# Build the c++ addons
cmake . -B build
cmake --build build

# Or, if you want to make it in parallel
cmake . -B build
cmake --build build -j $(nproc 2>/dev/null || sysctl -n hw.ncpu 2>/dev/null)
```

## Publish the package to NPM

If you want to publish you own version of the node package to NPM, you must need to update the [package.json](./package.json) to your own configuration, and change the host config to your own repo. See the following [link](https://github.com/bchr02/node-pre-gyp-github) for details about that.

```bash
# Link the c++ addons to the node package
./node_modules/.bin/node-pre-gyp package 

# Publish the release package to GitHub
set NODE_PRE_GYP_GITHUB_TOKEN=${GITHUB_TOKEN} 
./node_modules/.bin/node-pre-gyp-github publish

# Publish the Node package to NPM
npm login
npm publish
```
## Testing

To test the Node.js bindings:

```bash
npm test
```

# FAQS

- How to get the GitHub  Access Token?

    Look at the official GitHub [documentation](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)