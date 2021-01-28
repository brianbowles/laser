# laser
Diogenes Laser Club Project


So one thing about lasers in TX is that for commercial uses they require a license from the state.

Rule #2) The only way to know about the laser club is to be in the laser club.


* * * *************** Existing projects that laser club has definite interest in ************************* * * *
A https://github.com/Grix/helios_dac This is Helios source code and only thing I have used to control projector code-wise.
	I have code I have written here that is a kludged up main() file. 
	I should functionize it at least and then commit it (where?)
	I have 4 controllers. 1 was considered backup. So far they have all worked. I can loan out.

B https://github.com/Volst/laser-dac This is the typescript dude who has a lot of the primatives that the C++ aka 
brendan-w/lzr repo doesn't. Lines are needed for calibration and also for the MAME/arcade vector games. HersheyFonts 
( https://en.wikipedia.org/wiki/Hershey_fonts )are also needed - it opens up 
a lot of txt communications which is a whole new set of possibilities.  We need to be able to parse ilda files which is
needed for basic C++ lib. This lets us import external artwork. SVG would be better, but we have .ild exporter software
already.

C https://github.com/brendan-w/lzr This is the dude I want to build upon. The problem is this guy uses the "etherdream" 
controller which uses ethernet but costs $200 instead of $100. I dunno, I went with the open-soure solutions which is "Helios"
and costs $100 per controller. I don't think code is really setup properly for new drivers (?)

D MAME software to kludge up to project vector games.


************* Potential projects that are not existing and likely to be worked on by laser club ***************

1) Gun range target system. I'm going to make some version of this. This actually strikes me as a viable product but not 
good for meeting chicks or expanding space. Good for capital.
This is my first project because it is really easy to do a MFP. (Minimum Fun Product). 
Basically you get the other stuff working and it is just a program that randomly displays bitmaps according to random() 
with a few other additions. Once the lib etc is written this can go forward easily.

That will require a .ild parser/renderer+ line/rectangle drawing for calibration. Also requires a portable power source for projectors.

2) Environment projections - Utilizing a camera, record the scenery and process it via openCV/potrace and put out
effects of some sort. This was the original inspiration and has a lot of "creativity" in the stuff you can do.
 a> Game of Life - 
 One issue here is that if the scene changes, the GOL has to be restarted. Also, with the right arrangement of triangles,
 the default GOL rules need their rules turned into float math. (Vertices have more than 4 neighbors)
   I > Using some sort of edge detection break it up into triangles/polys and put images on it.
     A> Use PCL (Point Cloud Library?) to keep track of static points that can move with the environment
   II> Draw squares somewhere for GOL. Could be on ceiling of basement. 
   
 b> Something similar to above but that actually responds to the environment changing.
   I> Using FLIR camera - Not purchased $270 and agreeing to some arms export shit you can get setup with this.
   Very low res but would be good to immediately pull out human interaction. https://store.groupgets.com/collections/flir-lepton-accessories/products/flir-lepton-3-5
   
   II> Using designated colors. Orange stick/bat which makes motion detection etc super easy via openCV (assumed)
   III> Other heuristics. Just parsing standard video. Likely based around openCV or possibly the pcl
   
   Could have a person impart "GOL" type code onto something like 2aI. Basically the lasers respond to whoever is
   in the scene. This is likely the coolest "cool" factor that isn't niche geek but questionable in value due to licensing
   required for all commerical events.

3) Old vector arcade games from 80s projected. Socially distanced social gaming. Basically take MAME and compile it.
 There is a vector.ini so I suspect most
of the vector code is passed through a certain layer. I downloaded it to a linux install but got a weird error after 100s of 
files in that suggested a bad commit. (??) Was going to try recompile under windows with stable version.
https://www.youtube.com/watch?v=W4REBVgm4Nc . I have the linux compile notes on telegram.


******************* Various things TBD - numbers refer to above projects ***************************

1) https://github.com/brendan-w/lzr <-- Get this working on Helios. The port is likely fairly simple but still laborious requires
bit of study to grok it and determine best way to proceed. Right now there is only 1 laser controller supported - Etherdream. I don't think
code was written to easily change controller. Would be nice to integrate changes into dude's project so Helios works and then
based ports off that ?.   
His optimizations work at the angle/speed level to some degree. This is needed to squeeze out max performance.

So with art you can engineer around limitations of  projector. This means all of 2 (environment projections). 
However, 3 (arcade emulations) is where problem occurs because in theory arcade game displays at fixed vectors/framerate. 
It might be possible to speed up the underlying emulated hardware in the MAME emulator, but that would affect overall
gameplay and not just higher framerate.

So one of the ways to fix that is to optimize the ordering of the vectors by optimizations of the point list.
THis is where this package really shines. So if you optimizse it at the scene level and one big list, you can break
that path up into N paths where N is the number of projectors.

There is a problem where the "blanking" points are added  bu things ported from the Typescript/laserdac

2)https://github.com/Volst/laser-dac <-- Port the linedrawing and other primativess mentioned elsewhere to run in C++.
The end of this file has this mapped out as to what needs to be done via crude dependency tree which sorts
the ports needed to be done. 

3) Get MAME to compile TBD.
4) Dig into it with a debugger and see if you can isolate MAME vector calls.


************ laser-dac notes ************

This has a list of source files and their dependencies. The first argument is what the class inherits from.
After that are all the depedencies. Everything at the highest level was given a 1, then 2  etc. For porting, they
should follow these numbers.
   
laser-dac/packages/draw/src/

+1 Color - ? RGB - Not sure where declared or if class is even needed. Perhaps just hardcode RGP
+1 Point - none
+1 Shape - none 
+1 Wait- Shape - Point,Color

2 Circle - Shape - Color, Point, Wait - **** no use for ??

+2 Line- Shape - Point, Color, Wait **** Basic functionality needed to draw rectangle or any sort of calibration software

+3 Path- Shape - everything ? **** Huge dependencies - not sure what depends on this ?? **** no use for ?? - needed by HersheyFont

+3 Rect- Shape - Color, Line, Wait *** Well, we would end up rewriting this on our own. whatever.

4 CubicCurve - Shape - Color, Point, Bezier *** no use for 

+4 HersheyFont- Shape - fs, Color, Point, Path ** needed for txt output and trolling times

?5 ilda- Shape - fs, fromByteArray, Section/ilda-reader, Color, Point **** Needed to read in artwork for targetting project

5 QuadCurve - Shape - Point, Color. ,Bezier.js *** no use for ??

? Svg- Shape - everything **** Large depencies *** no use for - bought plugin

? TImeline- Shape **** Maybe we do animation in the future but **** no use for ??

Scene - none


laser-dac/packages/draw/src/transformers/

distort - 
rotate

-------------------
