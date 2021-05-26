Freeform camera voice, HP Omnicept Eye Tracking, UIX & Thumbnail optimizations
Hello everyone and welcome to another of our weekly updates!

With the first phase of desktop complete, we have focused on clearing up a bunch of smaller additions, tweaks and bugfixes. The desktop Freeform camera now automatically outputs your voice when applicable and we added more notes for accessing the user's viewpoint and camera state.

We added support for the HP Reverb G2 Omnicept Edition eye tracking, if you have this headset, your gaze, eye openness and pupils should now be mapped to your avatar out of the box! More of our native dependencies have been moved to Azure Pipelines and updated to latest version too, notably Opus, improving voice and music compression quality and Freetype, fixing bugs and adding WOFF font format support.

For some quality of life improvements, LogiX itself received new nodes, particularly for bitwise masks working with vectors and the Material tooltip can now batch convert materials to another type. We have also fixed numerous bugs, crashes and made security improvements, details are in the post below!

And last, but not least, we did some significant optimizations to the session thumbnail system, reducing CPU, memory and network bandwidth usage, as well as making more related optimizations for UIX panels, which overall improve smoothness of the experience and reduce a lot of cases of microstutters.



Games of Neos livestream
On our last regular Friday livestream, we went back to a bunch of the fun worlds created by our community and played a bunch of games! If you missed it, check out the footage below, as we drive around some race tracks, figure out who the murderer is in MurderX, talk to ghosts to figure out meme pictures and more!



HP Omnicept Eye Tracking Support
With the release of the HP Reverb G2 Omnicept Edition headset, we have integrated the SDK to provide full eye tracking support for anyone owning this headset. This includes the eye direction, eye openness (closing your eyelids) and pupil size.

Thanks to our generalized input system, there’s nothing extra that you need to do on your end! If you own the headset and have the Omnicept runtime installed, simply starting Neos and using any avatars already setup with eyes will work out of the box, just like with the Vive Pro Eye headset.

At the moment there doesn’t seem to be an API available for the lip tracking functionality, once that is made available to developers we’ll be happy to integrate it as well, as providing more options for our users for expression is an important aspect of Neos.

Voice output for Freeform Camera and more desktop polish
We’ve also made some last additions and polish to our new desktop mode. Most notably, the Freeform Camera will now automatically output your voice for other users if they’re sufficiently far away from your actual avatar and the camera is closer, making it easier to communicate.

New LogiX nodes for accessing various desktop information were added as well, allowing you to find out where the actual user’s viewpoint is, whether their Freecam is active and whether it’s voice is currently active too.

If you have a customized view visual, you will need to set up the AudioOutput and AvatarAudioOutputManager as you’d on the rest of your avatar, but check the “IsViewVoice” field on the latter component, so it gets activated properly.

Also note that Whisper mode won’t work in the Freecam mode to avoid some weirdness (both your avatar and camera can be potentially heard), you’ll need to switch back to first or third person for this mode.

Updated Opus (audio encoding) and Freetype (WOFF font support)
Thanks to our ongoing transition to Azure Pipelines, we were able to upgrade more of our native dependencies to their latest versions easily. Among the libraries that we moved were Opus and Freetype, responsible for audio encoding and font decoding respectively.

We have upgraded the Opus library to latest 1.3.1 from 1.1.3, which includes several years worth of improvements and bug fixes and should help improve the audio quality, especially at lower bitrates. This library is used to encode both your voice over network, as well as any desktop audio with our audio streaming feature.

Freetype library is used internally for decoding font files for the text rendering system and has been updated to 2.10.4 from 2.10.0. This mainly includes bug fixes, including an important security patch. However we also now added support for the WOFF font format support, alongside TTF and OTF.

Currently WOFF2 isn’t supported yet, despite the Freetype library supporting it, because it requires additional dependencies, but we’d like to eventually add support for this as well.

New LogiX operator nodes
Based on a few requests, we have expanded the LogiX nodes to include new operators and overloads as well to make it easier to work with vectors and bitmasks in particular. Boolean vectors (bool2, bool3 and bool4) now work with the bitwise logical operators as well as bit-shifting and bit-rotating nodes.

Integer vectors like int2, int3, int4, long3, long4 and so on now support bitwise operations as well. Any vectors can also be used with comparison operators, producing boolean vectors. E.g. comparing two float3 values will result in bool3 with each element of the vector being compared individually.

Batch Material Conversion
As a quality of life improvement, we added the ability to batch convert materials in the hierarchy to another type. Previously you could use the Material Tooltip to convert a single material, but oftentimes users would need to convert multiple materials in the scene or object/avatar.

In the latest version of Neos, you can simply grab a slot reference in the inspector with the Material Tooltip and you’ll see a new option in the context menu “Convert All To…”. Running this will convert all the materials within that hierarchy to a particular type.

As another quality of life, we also made the extraction process for all the materials in the hierarchy undoable, so you don’t have to manually delete all the orbs if you do it by mistake.



Mipmap Generation Control
Another quality of life improvement is now the ability to control mipmap generation for 2D textures. You can turn the mipmaps completely off for textures that you know won’t require them, such as skyboxes, saving some VRAM usage in the process.

For textures that do need mipmaps, you can now choose which rescaling algorithm will be used to generate them. You can currently pick between Bilinear, Box (default) and Lanczos3. The last filter in particular can be useful for certain textures and images, as it will produce sharper visuals at a distance, particularly with contrasting detail.



Updated Account / OAuth 2.0 website
The work in progress account and OAuth 2.0 website has received a small lick of paint courtesy of our new team member ProbablePrime and has been moved to a nicer domain https://auth.neos.com by Karel, in order to give it a more official look.

Alongside this we have also moved the main API endpoint to api.neos.com to help unify things a bit more, so if you have any 3rd party applications, make sure to update! The old endpoint will continue to function for a while, but will drop at some point in the future.



Optimized UIX and Thumbnail System
While investigating random hitches and stutters for the upcoming MTC Streamer room, we found a few underlying issues with the session thumbnail system and UIX and some related subsystems.

In the latest build we have implemented several different optimizations which overall should help reduce CPU, memory and network usage and provide smoother experience. Parts of the thumbnail system were removed to avoid overloading the system resources, particularly when just starting the session.

Headless servers will now also avoid compressing and reuploading thumbnails taken from other users or the default world thumbnail and any thumbnails are also registered with local cache, to avoid cases of having to redownload different instances of the same image or even own uploaded thumbnail in some cases.

We discovered that even disabled UIX canvases were being computed in the background, resulting in unnecessary CPU and memory usage. In the latest build, the UIX Canvas will defer any computations until it’s activated again. This way any inactive dash screens, closed dash, context menu or locally hidden/culled interfaces in the world will have minimized performance impact.

A few other smaller optimizations include directly loaded textures (thumbnails being a prime example) skipping metadata computation since it’s not necessary and any thumbnail updates in the UI being deferred until it’s activated as well.

We’re already getting reports of reduced microstutters and higher framerates on the latest build, so we’re happy that those changes are being felt despite being relatively small!

Fixed text layout glitches and flickering
Among many smaller bug fixes, one of the notable ones (if only for the few hours it took to narrow down) is the auto-size layout for the text rendering system. This has been actually caused by two major underlying issues, which have both been corrected.

Those combined caused auto-sized text to randomly spaz out and glitch when being changed and some words to get forcefully wrapped to multiple lines. With the bugfix in place, the text rendering system should now be more robust and produce more reliable results.



Favorite avatar security improvements
To improve general security of your inventory and particularly favorite avatars, we have reworked the mechanism by which your favorite avatar is loaded in worlds. Instead of the record being marked public and potentially accessible by anyone who knows its exact ID, a temporary one-time key is generated instead just prior to spawning.

While this doesn’t completely eliminate the possibility of ripping (that’s unfortunately not technically possible to prevent completely), it should significantly mitigate the possibility of one type of attack and provide better security hygiene by not marking avatar records as public.

We’ll be automatically unflagging those avatars as public soon as the new system proliferates, unless they are present in a public folder.

Various crash bugfixes & better Unicode handling
Other notable bugfixes from last week include numerous security improvements and some sources of crashes and freezes. A regression in network encoding for a few less commonly used types was patched, which resulted in worlds crashing on use.

Another notable fix is also for pasting invalid Unicode strings from the clipboard, for example when deleting half of Unicode character surrogate for certain symbols like 🔆. Previously doing this would freeze Neos until restart, although it would only impact the user executing this action and couldn’t be used for an attack.

In tandem with this, we improved the handling of Unicode strings for the text editing in Neos, which is now aware of those surrogate pairs and will treat them as a single symbol, rather than two independent ones.

bobool3ol

No two bobool3ol's are the same. They're always 7.


What’s next
With a bunch of smaller tasks, improvements and bug fixes out of the way and the first phase of desktop support nearly complete, I’ll be now taking a roughly week long break as mentioned in the last weekly update, to reset and refresh before tackling next major development tasks.

This unfortunately means that for the following week there will be no Neos updates, responses to questions and issues (from me anyway) and no weekly update next week, unless there’s an emergency. You’ll still be able to reach other team members in case you need assistance.

I hope that everyone will have a great week regardless. And as usual, big thanks to everyone for your support! Without you we wouldn’t be able to keep improving Neos every day! We’ll see you soon, with more big tasks and features to tackle!

You can find what's planned at our GitHub roadmaps and watch the latest developments on our official Discord server.
