# laser

Diogenes Laser Club Project

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


