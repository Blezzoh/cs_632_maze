# cs_632_maze
structure of a folder and some mini documentations


## /examples

### /experiments
1. json files: maps of network routes, includes network order.
2. txt files: have some coordinates. Don't know what **PREFIX** is for(might make sense later)

### /master_sat_configurations
1. json files of some sattelites routes

### /rtts
it has a lot txt files. I know rtt stands for round trip time. so I am assumtiong that each "point1/point2" has a correspondig rtt at the bottom of each text in milliseconds



## /rtt_simulator

from the main "readme.md": 

***A python framework for creating mixed network simulations. Currently, only LEO satellite networks and fiber networks are implemented, with LEO network hops being simulated using the subsimulator Hypatia(https://github.com/snkas/hypatia), and Fiber routes being simulated using knowledge of real world ping times. MAZE can be modified to support a variety of other network types(i.e. LTE) by simply choosing a subsimulator and integrating it into MAZE by implementing a new NetworkSimulator child class. The framework provides all the building blocks needed to write mixed network simulators with both ground and LEO space components for different situations(license: MIT)***

#### 1. constellation_config.py(ConstellationConfig):
```
    ConstellationConfig is a structure defining what a LEO satellite constellation
    looks like

    :param str name: The constellation identifier
    :param float eccentricity: The eccentricity of the satellites
    :param float arg_of_preigee_degree: The degree of the arg of preigee
    :param bool phase_diff: Whether to use phase diff or not
    :param float mean_motion_rev_per_day: Mean motion revolutions per day
    :param int altitude_m: Altitude of the satellites in meters
    :param float max_gsl_length_m: Max ground station link length in meters
    :param float max_isl_length_m: Max inter-satellite link length in meters
    :param int num_orbs: Number of orbits
    :param int num_sats_per_orb: Number of satellites per orbit
    :param int inclination_degree: The inclination degree
```

#### 2. net_point.py(NetworkPoint):
```
NetworkPoint describes a node that is a part of the simulation network

    :param str name: The identifier of the node
    :param str type: The type of node. Either constants.CITY_POINT_TYPE or
        constants.GS_POINT_TYPE
    :param float lat: Latitude of the node
    :param float long: Longitude of the node
```
#### 3. net_segment.py(NetworkSegment) 
```
NetworkSegment describes a connection between two nodes on the network
    path

    :param net_point.NetworkPoint net_point_a: The start node in the segment
    :param net_point.NetworkPoint net_point_b: The end node in the segment
```

#### 4. network_simulator(NetworkSimulator)

has an abstract method generate_rtts.

```
NetworkSimulator is an interface describing a class which can generate
    Round Trip Times between two points
```

#### 5. sar_relay_sim (SatelliteNetworkState)

how does this relate to hypatia framework that works for linux systems? 


```
SatelliteNetworkState generates a LEO satellite constellation using the
    Hypatia framework

    :param constellation_config.ConstellationConfig constellation_config: An
        object describing the satellite constellation
    :param gs_points: A generator object the yields net_point.NetworkPoint objects
    :param int duration: Duration of the simulation in seconds
    :param str output_dir: The folder to save the generated state to
```

#### 6. terrestial_simulator (DistanceBasedPingCalculator)

```
DistanceBasedPingCalculator estimates the Round Trip Time between two
    points based on the known Round Trip Time and distance between two
    master points

    :param float distance_km: Distance between the two master points in kilometers
    :param float rtt_ms: Round Trip Time between the two master points
```

## /satgen
0. Should read whatever the hypatia paper is
1. this is the replica of "/satgen" in hypatia
2. there is a bit of luck because this has a readme. I added it to the repository
3. there are some required installation in the readme
4. BIG NOTE: this runs on linux. which gives me an impression that this entire codebase is meant for linux as well.
This also explains why hypatia is also on linux

## /test

haven't spent a time looking into this. All I know these are some test files

## /tools

From the main readme:

***A set of python tools used for the University of Oregon's research into how the internet can be made more resilient in the event of natural disasters that damage routing infrastructure. `pnw_rtt` is a tool that can perform simulations of mixed networks along the west coast and provide estimated Round Trip Times of such networks(license: MIT)***


#### /tools/pnw-rtt/pnw-rtt.py

This files runs a simulation it has a nice help flag, this is the output:

```
Simulate RTT time for a mixed ground-satellite network in the PNW                                                                                                                                                                               positional arguments:          
config           simulation config file                                                                                
path             folder to run simulation in and store generated state                                                                                                                                                                        options:                                                                                                                 
 -h, --help       show this help message and exit                                                                        
-g, --gen-state  generate the required network state                                                                    
-r, --run        run simulation                                                                                        
-a, --all-pairs  Calculate RTT between all pairs of nodes not just a simple path 
```


#### /tools/gen-config/gen-exp-config.py

```
Generate simulation config for sector based experiment

positional arguments:
  sector_config  config file location
  output_config  generated config file

options:
  -h, --help     show this help message and exit
```
