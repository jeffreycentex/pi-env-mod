Raspberry Pi Environmental Monitor
==================================

This is the repository for my Raspberry Pi Utility Enviornmental Monitor.

I basically have a Raspberry Pi setup using GPIO to monitor the temperature
of two freeers, one refrigerator, and two room monitors.  The script logs 
the temperatures to a RRD database, runs a graph job every 15 minutes to
update the graphs, and then toggles a series of 3 LED's that will be 
triggered if the temperature has reached an un-safe condition.  There is
a swich under each LED that will toggle the light off.  

Requirements:
-------------
  * A series of DS 18B20 One-Wire temperature sensor
  * A Raspberry PI
  * A version of Rasbian after December 2012 with One-Wire Support



RRD Setup
---------

My current RRD database was setup as follows:

    rrdtool create envmonitor1.rrd \
    --start now-30s --step 60 \
    DS:utilroom:GAUGE:120:-50:150 \
    DS:garageroom:GAUGE:120:-50:150 \
    DS:chestfrz:GAUGE:120:-50:150 \
    DS:utilfrz:GAUGE:120:-50:150 \
    DS:utilref:GAUGE:120:-50:150 \
    RRA:MIN:0.5:15:96 \
    RRA:MAX:0.5:15:96 \
    RRA:AVERAGE:0.5:5:288 \
    RRA:AVERAGE:0.5:60:4200 \
    RRA:LAST:0.5:1:60 \
    RRA:LAST:0.5:5:288

You can adjust this script as necessary to add/remove sensors (the DS: lines).
