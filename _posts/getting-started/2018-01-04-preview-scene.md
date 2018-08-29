---
date: 2018-01-01
title: Preview your scene
description: What you can see in a scene's preview
redirect_from:
  - /documentation/preview-scene/
categories:
  - getting-started
type: Document
set: getting-started
set_order: 3
tag: introduction
---

Once you have [built a new scene]({{ site.baseurl }}{% post_url /getting-started/2018-01-03-create-scene %}) ) or downloaded a [sample scene]({{ site.baseurl }}{% post_url /examples/2018-01-08-sample-scenes %}) ) you can preview it locally.

## Before you begin

Please make sure you first install the CLI tools. In Mac OS, you do this by running the following command:

```bash
npm install -g decentraland
```

See the [Installation Guide]({{ site.baseurl }}{% post_url /getting-started/2018-01-01-installation-guide %}) for more details and specific instructions for Windows and Linux systems.

## Preview a local scene

To preview a local scene run the following command on the scene's main folder:

```bash
dcl start
```

Any dependencies that are missing are installed and then the CLI opens the scene in a new browser tab automatically. It creates a local web server in your system and points the web browser tab to this local address.

Every time you make changes to the scene, the preview reloads and updates automatically, so there's no need to run the command again.

## Preview a remote scene

To preview a remote scene you must first start a WebSockets server in your local machine. To do this:

From the scene directory run the following bash commands:

```bash
cd server

npm install
# npm will find your dependencies

npm run build
# npm will build the socket server

npm start
# now the port is running
```

Note that when the server is running, the command informs you what port that the server is running on, take note of this. It will probably be 8087.

Check that the _scene.json_ file in your scene has the same websocket port set up, otherwise change the file so that it matches the local server you're running.

Once the websocket server is up and the scene is properly pointing at it, fire up the preview as you would with a local scene.

From the scene directory run the following bash command:

```bash
dcl start
```

Any missing dependencies are installed and then the CLI opens the scene in a new browser tab automatically.

You can point other browser tabs to the same local address and this will add new users to the scene. User's aren't able to see other avatars in the preview, but they can see a marker representing other users. If the actions of a user affect the scene state, which is hosted on the server, all users should see the effects of this.

> Note: Currently, the preview of remote scenes doesn't automatically update changes to the scene as local scenes do. Each time you make changes, you must run the build and start commands for the server again. It's advisable to start building a scene as local and only move its code to a remote scene once it's mostly complete.

## Upload a scene to decentraland

Once you're happy with your scene, you can upload it and publish it to Decentraland, see [publishing]({{ site.baseurl }}{% post_url /getting-started/2018-01-07-publishing %}) ) for instructions on how to do that.

## Parameters of the preview command

You can add the following flags to the `dcl start` command to change its behavior:

- `--no-browser` to prevent the preview from opening a new browser tab.
- `--port` to assign a specific port to run the scene. Otherwise it will use whatever port is available.

<!--
Seems to be removed:

- `--skip` to skip the confirmation prompt.
-->

> To preview old scenes that were built for older versions of the SDK, you must install the latest versions of the `decentraland-api` and `decentraland-rpc` packages in your project. Check the CLI version via the command `dcl -v`

## Basic usage of the scene preview

Running a preview provides some useful debugging information and tools to help you understand how the scene is rendered. The preview mode provides indicators that show parcel boundaries and the orientation of the scene.

When viewing a preview, you can press the Esc key to disengage the mouse and use it normally.

If your scene outputs messages to console (using `console.log()`) you can view these messages as they are generated by opening the JavaScript console of your browser. For example, when using Chrome you access this through `View > Developer > JavaScript console`.

The upper-left corner of the preview informs you of the following:

- _FPS_ : frames per second
- _MS_ : network ping delay in milliseconds
- _MB_ : memory usage in megabytes

Click on the graph to switch through these metrics.

## Preview scene size

The scene size shown in the preview is based on the scene's configuration, you set this when building the scene using the CLI. By default, the scene occupies a single parcel (10 x 10 meters).

If you're building a scene to be uploaded to an estate that occupies more parcels than the preview shows, you can edit the _scene.json_ file to reflect this, listing multiple parcels in the "parcels" field.

```json
 "scene": {
    "parcels": [
      "0,0",
      "0,1",
      "1,0",
      "1,1"
    ],
    "base": "0,0"
  },
```

## Debug a scene

If the scene can't be compiled, the scene in your browser will be rendered as a large white box that occupies the entire space of the scene.

If this occurs, there are several places where you can look for error messages to help you understand what went wrong:

1.  Check your code editor to make sure that it didn't mark any syntax or logic errors.
2.  Check the output of the command line where you ran `dcl start`
3.  Check the JavaScript console in the browser for any other error messages. For example, when using Chrome you access this through `View > Developer > JavaScript console`.
4.  If you're running a preview of a remote scene, check the output of the other command line window, where you ran `npm start` to start the server.

If your scene excedes any of the [scene limitations]({{ site.baseurl }}{% post_url /sdk-reference/2018-01-06-scene-limitations %}), for example if there are too many triangles in it, the scene won't be rendered. If this occurs, a warning sign will be rendered in the space where your scene should be, detailing the problem.

If an entity is located or extends beyond the limits of the scene, it will flash with red color to indicate this. Nothing in your scene can extend beyond the scene limits. This won't stop the scene from being rendered locally, but it will stop it from being deployed to Decentraland.

You can also add `console.log()` and `console.trace()` commands to your code so that they print information to the JavaScript console.

You can also use the `sources` tab in the developer tools menu to add breakpoints and pause execution while you interact with the scene in real time.

## View collision meshes

While viewing the preview, you can press `c` to view any collision meshes loaded in the glTF models of the scene. These are usually invisible, but determine where an avatar can move through, and where it can't.

![](/images/media/collision-meshes.png)

Collision meshes can be added to any model in an external 3D modeling tool like Blender. Large models like houses often include these, they are usually a lot simpler geometrically than the original shape, as this implies much less computational requirements. Stairs typically use a simplified collision mesh like a ramp to make it easier to climb, otherwise a character would have to jump up every step.

## View bounding boxes

While viewing the preview, you can press `b` to view any bounding boxes. Bounding boxes show the space occupied by an entity, it's especially useful to see these when dealing with invisible entities.

## Using the Ethereum test network

If your scene makes use of transactions over the Ethereum network, for example if it prompts you to pay a sum in MANA to open a door, you can avoid using real currency while previewing the scene.

For this, you must use the _Ethereum Ropsten test network_ and transfer fake MANA instead. To use the test network you must set your Metamask Chrome extension to use the _Ropsten test network_ instead of _Main network_. You must also own MANA in the Ropsten blockchain, which you can aquire for free from Decentraland.

To preview your scene using the test network, add the `DEBUG` property to the URL you're using to access the scene preview on your browser. For example, if you're accessing the scene via `http://127.0.0.1:8000/?position=0%2C-1`, you should set the URL to `http://127.0.0.1:8000/?DEBUG&position=0%2C-1`.

Any transactions that you accept while viewing the scene in this mode will only occur in the test network and not affect the MANA balance in your real wallet.