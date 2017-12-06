# SFTW README.MD

Roles:------------------------------------------------------------------------------------------------------------------------------

Designer

Developer

Researcher

Paragraph:-------------------------------------------------------------------------------------------------------------------------


This is my repository for Patrick Pagano's Scripting for the Web Class

For this project I chose to connect the Nexus UI to Tone.js in order to piece together an interesting Graphical User Interface 
for sound using a Javascripting language. I knew that tackling this scope of a project would prove difficult but
I was fairly hopeful that I would be able to make something awesome. After a semester long process, I wasn't able to do everything
I hoped and dreamed I'd be able to from when I started, but I learned a lot along the way especially in terms of how a Javascript language 
functions (in this case Tone.Js). I made the decision not to enhance the UI and make it prettier because I felt the design was actually
captivating for something on a mobile device. My intentions for the project, in theory, were very interesting "Have a bunch of dials/sliders, that change a sound/manipulate it, and make it look pretty. To a great degree I've accomplished that, even if just looks like a Prototype.

I started this project by downloading the repository for Nexus and Tone.Js from the Github Patrick provided on blogger
https://github.com/nexus-js/ui/. I then proceeded to generate my own html file that would include a bunch of different GUI elements
from Nexus and I would later connect those to Tone.js. I was very ambitious until I failed repeatedly. I then tackled a smaller struggle
that would be editing an existing example file from the Github Patrick provided, and to my surprise I was able to actually get some things done. I wouldn't say I "Made" something amazing, because I simply added elements to a file that had already been created, but I was able to generate some code from scratch simply to get parts of the Nexus UI to work with Tone.js and for that I am very happy with myself.

In this Github I have included my SFTW folder and file called (demo.html --- located within the example folder, that's located within the UI-master folder), for the review of Patrick Pagano for my Final Project/Class Grade.

I hope you see that I made an effort, and that although I may have failed to achieve what I hoped to achieve, I was relentless along the way.

RAW HTML CODE:------------------------------------------------------------------------------------------------------------------

<!doctype html>
<html>

<head>
  <meta charset="utf-8" />
  <title>Matthew Castillo's SFTW PROJECT</title>
  <script src="../dist/NexusUI.js"></script>
  <script src="js/Tone9.js"></script>
  <meta name="viewport" content="width=200, initial-scale=1">
</head>
<body style="margin:0;padding:0;">


  <div style="font-size:20px;padding:20px;text-align:center;margin-top:40px;">
    CASTILLO NEXUS & TONE
  </div>

  <button id="mobileStart"></button>

  <div id="drone" style=>
    <div style="background-color:#000;border:solid 4px #B00;width:300px;padding:20px;margin:0 auto;">
      <div>
        <div nexus-ui="toggle" id="power" style="width:60px;height:37.5px;float:left;margin:0"></div>
        <span style="height:37.5px;line-height:42px;margin-left:15px">Power On</span>
      </div>
      <div style="overflow:auto">
        <div style="padding:15px 0px 0px;width:300px;overflow:auto">
          <div nexus-ui="slider" id="timbre" style="width:200px;height:37.5px;display:inline-block;"></div>
          <div style="display:inline-block;float:left;padding:4.5px 105px 0px">Timbre</div>
        </div>
        <div style="padding:22.5px 0px 0px;width:300px;overflow:auto">
          <div nexus-ui="pan" id="pan" style="width:300px;height:37.5px;display:inline-block;"></div>
          <div style="display:inline-block;float:left;padding:4.5px 120px 0px">Pan</div>
        </div>

        <div style="padding:22.5px 0px 0px;width:300px;overflow:auto">
          <div nexus-ui="position" id="filter" style="width:300px;height:225px;display:inline-block;"></div>
          <div style="display:inline-block;float:left;padding:4.5px 105px 0px">Filter</div>
        </div>
      </div>

      <div>
        <div nexus-ui="spectrogram" id="spectrogram" style="width:340px;height:225px;margin-left:-20px;margin-bottom:-30px;"></div>
      </div>

      <div style="padding:22.5px 0px 0px;width:300px;overflow:auto">
          <div nexus-ui="Dial" id="verb" style="width:300px;height:225px;display:inline-block;margin-top:30px;"></div>
          <div style="display:inline-block;float:left;padding:4.5px 75px 0px">Room + Wet</div>
      </div>

    </div>

  </div>


<style>

body {
  font-family:Helvetica;
  font-weight:300;
  background-color:#868686;
  color:#FFF;
  font-size: 20px;
}
[nexus-ui] {
  margin:0px;
}
#mobileStart {
	background: transparent;
    border: none !important;
    font-size:0;
}
</style>
</body>
<script>

  Nexus.context = Tone.context;

  mobileStart = document.getElementById('mobileStart')
  mobileStart.addEventListener('touchend',function() {
    var osc = Nexus.context.createOscillator()
    osc.connect(Nexus.context.destination)
    osc.start(0)
    osc.stop(0.1)
    Nexus.clock.start();
  })

  Nexus.colors.accent = "#FF0000";



  drone = new Nexus.Rack('#drone');

  droneSynth = {
    fm: new Tone.FMOscillator(20, "sawtooth", "square").start(),
    fm2: new Tone.FMOscillator(140, "sawtooth", "sawtooth").start(),
    vol: new Tone.Volume(-Infinity),
    pan: new Tone.Panner(0),
    filter: new Tone.Filter(100, "lowpass"),
    verb: new Tone.Freeverb({}),
    compressor: new Tone.Compressor(-100, 20)
  }

  droneSynth.fm.connect( droneSynth.filter )
  droneSynth.fm2.connect( droneSynth.filter );
  droneSynth.filter.chain( droneSynth.compressor, droneSynth.vol, droneSynth.pan, droneSynth.verb, Tone.Master)


  droneSynth.fm.harmonicity.value = 4
  droneSynth.fm2.harmonicity.value = 4


  drone.power.on('change',function(v) {
    if (v) {
      droneSynth.vol.volume.rampTo(-20,1)
    } else {
      droneSynth.vol.volume.rampTo(-Infinity,1)
    }
  })

  drone.timbre.min = 10
  drone.timbre.max = 20
  drone.timbre.on('change',function(v) {
    droneSynth.fm.modulationIndex.rampTo(v,0.1)
    droneSynth.fm2.modulationIndex.rampTo(v,0.1)
    console.log(v);
  })
  drone.timbre.value = 0


  drone.pan.on('change',function(v) {
    droneSynth.pan.pan.value = v.value;
    console.log(v);
  })



  drone.filter.minX = 0
  drone.filter.maxX = 1400
  drone.filter.minY = 0
  drone.filter.maxY = 10

  drone.filter.on('change',function(v) {
    droneSynth.filter.frequency.value = v.x;
    droneSynth.filter.Q.value = v.y;
    console.log(v);
  })



    droneSynth.verb.wet.value = 0.2


    drone.spectrogram.connect(Tone.Master);
    drone.spectrogram.colorize("fill","#fff")
    drone.spectrogram.colorize("accent","#FF0000")

    drone.verb.on('change',function(v) {
    droneSynth.verb.roomSize.rampTo(v,0.1);
    droneSynth.verb.wet.rampTo(v,0.1);
    console.log(v);
  })


</script>
</html>


Dependencies:--------------------------------------------------------------------------------------------------------------------

Nexus UI https://nexus-js.github.io/ui/

Tone.js  https://github.com/nexus-js/ui/
