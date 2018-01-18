# mythbrake
A script to transcode mythtv ts files to mpeg4 using handbrake cli and.. bash...

## Getting Started

First, you need the following command line utilities:

  * HandBrakeCLI
  * mediainfo

You can use your package manager to install mediainfo. handbrakecli typically is not available directly from most distros, but i think you can figure out how to get it. 

Next, you need to edit the script in the section marked SOME "CONSTANTS FOR USER EDITING"

To use this script, put it somewhere with permissions so that mythtvbackend can read/execute it. Then, in the backend configuration, add the following as a user job:

 `/path/to/mythbrake.sh "%DIR%" "%FILE%" "%CHANID%" "%STARTTIMEUTC%" "%TITLE%" "%SUBTITLE%" "%CATEGORY%"`
 
The "General" section is where mythtv 0.28 backend puts user job definition, so look under there. 
 
Then, when you schedule a recording in the frontend, do not bother to transcode or mark commercials, but instead choose to invoke user job #1 on the file(assuming you defined this job as user job 1). Some frontend user interfaces may not have these options, so it may be best to use the built in mythtv-frontend application. You may also consider changing the skin of the frontend ui, as the default one hides a bunch of the full explanations for what recording options there are. 

## Notes on operation

This script simply cuts commercials and transcodes the mpeg transport stream to mp4, for HD and SD content. It does NOT replace the old transport stream file.

There is a SD channel filter array for conditionally invoking commercial flagging, but it is set up for german tv. 

This script is currently configured to run on an ubuntu/debian like distro. you may need to change some pathing for other distros. 

This transcoding is SLOOOOWW on HD content on my terrible dual-core AMD machine. 

this script can send email, if sendmail is configured on your machine.

## Source Formats

**HDTV** will be reencoded with Mpeg4 AVC H264 to save space. This script runs mythcommflag on the content before transcoding.

**SDTV** will have commercials cut out if necessary and will then be transcoded to Mpeg4 ASP (H263, Xvid, DivX). The script will check the ChanID of the recording to decide wethere there are commercials, so you have to adapt the list of ChanIDs to your setup. If the ChanID indicates the source file contains commercials, I use Mpeg4 ASP instead of Mpeg4 AVC on SDTV recordings because the transcoding is much faster and it is easier to cut later on as mythcommflag isn't perfect. For manual cutting afterwards I use Avidemux. If no commercials are present, the SDTV recordings are transcoded to H264 as well.

**Audio** You can pass this script up to three languages and corresponding descriptions. The script will then automatically search the source file for the best track (i.e. the track with the most channels, e.g. 5.1 is better than stereo) for each given language. Surround sound is preserved, tracks with more than 2 channels are passed through without audio transcoding, mono or stereo tracks are transcoded to 128kbits/sec. 

## Debug

you can run this script outside the context of mythtv, as the mythtv user, by invoking it directly on the command line, like so: 

`/media/backup/tv/mythbrake.sh "/media/backup/tv" "1041_20180114000000.ts" "1041" "20180114000000" "Name of resulting file" "" ""`

## Thanks

This script came from [https://www.mythtv.org/wiki/Mythbrake](https://www.mythtv.org/wiki/Mythbrake)
