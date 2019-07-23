# Install and Run Notes
## MacOS


### Installing

So, somehow I got it installed and I’m not really quite sure how that happened. 

Followed roughly the first half of the instructions [here](https://gist.github.com/jyt109/76eba9b502e2c90bb728) to get it running. When trying to follow the quickstart guide [here](<https://github.com/Project-OSRM/osrm-backend/wiki/Building-OSRM>), it seems like the "mason" folder isn't created. But, once I install all of the dependencies etc., I then run the 

```
cmake ../ -DENABLE_MASON=1
make
```

lines and it seems to have worked. In fact, I know it works because when I go to start it up following the instructions [here](<https://github.com/Project-OSRM/osrm-backend/wiki/Running-OSRM>), it does in fact work. The final code I ran should look something like this: 



#### Install example code

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



### Running

I first tried running these commands from the [wiki](<https://github.com/Project-OSRM/osrm-backend/wiki/Running-OSRM>).

Simple checklist that will make sense after reading throught the code and comments. 

* Double check that the file names are correct. 
* You actually have to use "./" before the commands to get them to run AND if things are floating around in different directories, you have to include that as well. 
  * May not actually need the "./" but it doesn't hurt. 



#### From the wiki

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



#### Actual commands

```
# This is the terminal input with the current directory set to the osrm-backend file directly downloaded from GitHub

./build/osrm-extract berlin-latest.osm.pbf -p profiles/car.lua
build/osrm-partition berlin-latest.osrm
build/osrm-customize berlin-latest.osrm
build/osrm-routed --algorithm=MLD berlin-latest.osrm
```

## Ubuntu

### Installing

Ubuntu password: stanfordsus

The following commands clone the repo I've created then run the install and first_run scripts. The ```chmod``` commands set the permissions. Could probably do it elsewhere but this seems easy enough. You will most likely be prompted for the ubuntu password and it's listed above.  

```
git clone https://github.com/maxo16/osrm_stuff
cd osrm_stuff
chmod 755 install_script
./install_script

chmod 755 first_run_script
./first_run_script

chmod 755 run_script
./run_script
```





Just following the instructions found [here](https://datawookie.netlify.com/blog/2017/09/building-a-local-osrm-instance/).

Can just copy and paste each line (may become a batch script later)

```bash
sudo apt update
sudo apt install -y git cmake build-essential jq htop
sudo apt install -y liblua5.2-dev libboost-all-dev libprotobuf-dev libtbb-dev libstxxl-dev libbz2-dev

git clone --branch v5.18.0 https://github.com/Project-OSRM/osrm-backend.git

cd osrm-backend/

mkdir build
cd build/
cmake ..

make

sudo make install
```



### Downloading OSM Data

Creating a map directory and changing to it to prevent clutter in the parent directory.  Run the ```wget``` function inside the map directory. 

```
mkdir map
cd map

wget -O test.osm "https://overpass-api.de/api/map?bbox=-121.5802,37.7419,-120.9698,38.1437"
```

### Creating the OSRM Object and Running

Run the lines below while still in the map directory. It creates all of the needed graph objects inside the directory that the input OSM file is located. By doing this, it prevents the parent directory from getting too cluttered and makes deletion of files easy to create another graph object. Although, if you just use a different name then it's not a problem. 

Imperfect explanation of what's goin on:

1st line:  creating an OSRM object from the data downloaded

2nd line: creating the contraction hierarchies (see the documentation on GitHub for explanation)

3rd line: runs OSRM. Note, only this line has to be run in the future to get OSRM up and running. Have to be inside of the map directory to run this line as is, otherwise modify extensions as appropriate. 

```
../build/osrm-extract test.osm -p ../profiles/car.lua
../build/osrm-contract test.osrm
../build/osrm-routed test.osrm
```




#### Future things to consider

* Figured out how to 