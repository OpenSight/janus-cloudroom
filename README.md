# janus-mixroom
janus-mixroom is a videoconferencing SFU plugin of Janus, which can mix all AV streams in a room and re-publish it to another media server. It's derived from the official videoroom plugin, so that it owns almost the same interfaces with videoroom, and the new stream mix interface as well.  In addition to videoconferencing SFU like video room, Janus-mixroom also implements the functions of stream decoding, mixing, encoding and re-publishing based on FFmpeg 2.8.x(http://ffmpeg.org/).  

**this project is under development now, it's unfinished. don't used me.**

## Dependencies

To install it, you'll need to satisfy the following dependencies:

* [Janus](https://janus.conf.meetecho.com/)
* [FFmpeg](https://janus.conf.meetecho.com/) (2.8.x version must be used, API of later version has been changed)


## Compile

After install the dependencies successfully, configure and compile as usual to start the whole compilation process:



	./configure --prefix=/opt/janus
	make
	make install

where prefix parameter must be the same with the Janus core configuration, otherwise, compilation would failed. 

Since Janus-mixroom requires configuration files like other Janus plugin, you'll probably also want to install the default configuration files to use, which you can do this way:

	make configs

Remember to only do this once, or otherwise a subsequent `make configs` will overwrite any configuration file you may have modified in themeanwhile.

## Configure and start

you can edit the config file for this plugin before start Janus, which locates at: 
    
    <installdir>/etc/janus/janus.plugin.mixroom.jcfg
    
After that, you can start the Janus server like usual, this mixroom plugin would be loaded automatically.
