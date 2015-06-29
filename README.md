VRML - Virtual Review Management Loop
=====================================

The design process in the AEC industry today requires many changes and constant
checkins with clients throughout the process. The faster these checkins can
occur, the more design iterations can be completed, leading to better
designs. The current process of sending models to clients via email or FTP and
scheduling calls or waiting for emails to receive feedback are slow and onerous.

Over the past few years Virtual Reality technology has been making tremendous
progress, and is no longer a toy of graphics and gaming enthusiasts. This
progress has not gone unnoticed by the AEC industry, and many are starting to
explore how VR software and equipment enables new and exciting ways of
interacting and exploring their designs.

A product of the [AEC Hackathon 2.3 - SoCal](http://aechackathon.com/aec-hackathon-socal/)
(Summer 2015), team VRML (Virtual Review Management Loop) explored ways for (A)
designers to share their models faster and (B) clients to virtually experience
those models. Out of this exploration came a process for virtual design review
based in the browser. In this process, designers can share their model with
clients via a URL, enabling clients to review the design easily from their
browsers - no additional software installations necessary. Then, as the client
asks for changes to the model, the designer can implement those changes to the
model (using their favorite design tool) in real time. The client can then
refresh their page (using the same url) to see the updated model with their
requested changes.


Technical details
-----------------
The sample workflow below helps outlines how this process occurs.

!!(assets/system_diagram.png)

The process starts with the designer, producing the model on their laptop in
Revit. When they're ready to get client feedback, they can export their model to
.obj format and upload that model to the server.

The designer can send a unique url to their client, which will direct them to
the server to load that .obj model along with html and javascript for rendering
it in their browser. The client can then explore the model with their mouse, or
with a VR headset (Occulus Rift DK2 or Google Cardboard) if they have one
available. This rendering is all done using Three.js library for WebGL.

As the client has feedback on the model, the designer can then begin modifying
the model and push a new version of the model up to the server for the client to
view again in their browser (after a refresh to load the new model). This
process can repeat as often as necessary, as fast as the client and the designer
can communicate.


This project is being posted to document how we accomplished this using various
existing technologies and stimulate follow on work. There is very little novel
or interesting technical contribution here, other than wrangling all the parts
together. This was a challenge in and of itself, since the Oculus Rift DK2 is
still very new and the WebVR is an evolving web API standard (still in draft
mode). To make the browser rendering work using ThreeJS, we hobbled together
resources from the following locations:

ThreeJS dev branch - base three.[min.]js file, plus OBJLoader, VREffects and VRControls.
VRPolyfill - Helpful polyfill for browsers and supporting cardboard on other
browsers [here](https://github.com/borismus/webvr-polyfill)
VRManager - Handy [utility](https://github.com/borismus/webvr-boilerplate) utility for
managing the vr experience

The browser VR experience is tested on the Rift DK2 and Google cardboard. We
used Dropbox to synchronize the model between the designer's machine and the
server, which was running a simple http static file server.

To run the code:
1 download the repository
2 replace the broken symlink for assets/model.obj with the actual model file
you want to serve (or a symlink to it) (Currently this code can only serve an
.obj file, but small tweaks could make it serve any format ThreeJS supports)
3 Run a server in the same repository (we used `python -m SimpleHTTPServer`)
4 Navigate to http://localhost:8000/ in a browser (you'll need a browser that
supports webVR if you want to use the VR capabilities. We used the custom
[chrome nightly branch with support for WebVR] (https://drive.google.com/folderview?id=0BzudLt22BqGRbW9WTHMtOWMzNjQ).
