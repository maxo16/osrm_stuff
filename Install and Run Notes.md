# Install and Run Notes



## Installing

So, somehow I got it installed and I’m not really quite sure how that happened. 

Followed roughly the first half of the instructions [here](https://gist.github.com/jyt109/76eba9b502e2c90bb728) to get it running. When trying to follow the quickstart guide [here](<https://github.com/Project-OSRM/osrm-backend/wiki/Building-OSRM>), it seems like the "mason" folder isn't created. But, once I install all of the dependencies etc., I then run the 

```
cmake ../ -DENABLE_MASON=1
make
```

lines and it seems to have worked. In fact, I know it works because when I go to start it up following the instructions [here](<https://github.com/Project-OSRM/osrm-backend/wiki/Running-OSRM>), it does in fact work. The final code I ran should look something like this: 



### Install example code

```
# from the gist
brew install boost git cmake protobuf libzip libstxxl libxml2 osm-pbf lua51 luajit luabind tbb
brew uninstall lua 
brew install GDAL                                          
brew install osmosis                                       
git clone https://github.com/Project-OSRM/osrm-backend.git 
cd osrm-backend                                            
mkdir -p build                                             
cd build                                                   

# from the osrm wiki
cmake ../ -DENABLE_MASON=1
make

```



## Running

I first tried running these commands from the [wiki](<https://github.com/Project-OSRM/osrm-backend/wiki/Running-OSRM>).

Simple checklist that will make sense after reading throught the code and comments. 

* Double check that the file names are correct. 
* You actually have to use "./" before the commands to get them to run AND if things are floating around in different directories, you have to include that as well. 
  * May not actually need the "./" but it doesn't hurt. 



### From the wiki

```
# This just downloads the osm file that everyone seems to use as an example. 
wget http://download.geofabrik.de/europe/germany/berlin-latest.osm.pbf

# The following functions extract the compressed file and get it read to run. Note that this is it appears on the website. 
osrm-extract berlin.osm.pbf

# On online post and the CH, the alternate version of the first line is as follows, this is the one you should probably use. When attempting to run the first version as is, you may get a message that says something like "car.lua" doesn't exist. 

osrm-extract berlin-latest.osm.pbf -p profiles/car.lua
osrm-partition berlin.osrm
osrm-customize berlin.osrm

osrm-routed --algorithm=MLD berlin.osrm 
```



### Actual commands

```
# This is the terminal input with the current directory set to the osrm-backend file directly downloaded from GitHub

./build/osrm-extract berlin-latest.osm.pbf -p profiles/car.lua
build/osrm-partition berlin-latest.osrm
build/osrm-customize berlin-latest.osrm
build/osrm-routed --algorithm=MLD berlin-latest.osrm
```



#### Future things to consider

* Creating the map that it uses to route creates a number of files. It actually isn't too clever but probably worth spending a minute to think about how to structure things such that it's all created in a separate file. At some point, I'm thinking this could get turned into some sort of batch script or something similar that we'd then use to automate the process. 