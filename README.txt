{\rtf1\ansi\ansicpg1252\cocoartf1404\cocoasubrtf110
{\fonttbl\f0\fswiss\fcharset0 Helvetica;}
{\colortbl;\red255\green255\blue255;}
{\*\listtable{\list\listtemplateid1\listhybrid{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat1\levelspace360\levelindent0{\*\levelmarker \{disc\}}{\leveltext\leveltemplateid1\'01\uc0\u8226 ;}{\levelnumbers;}\fi-360\li720\lin720 }{\listname ;}\listid1}}
{\*\listoverridetable{\listoverride\listid1\listoverridecount0\ls1}}
\margl1440\margr1440\vieww10800\viewh8400\viewkind0
\pard\tx720\tx1440\tx2160\tx2880\tx3600\tx4320\tx5040\tx5760\tx6480\tx7200\tx7920\tx8640\pardirnatural\partightenfactor0

\f0\fs24 \cf0 SNLP: Systematic Nonlinear Planner \
\
The tests and outputs are for my implementation of SNLP for the following problem. The tests are finished in milliseconds. \
\
\pard\pardeftab720\sl340\sa240\partightenfactor0
\cf0 \expnd0\expndtw0\kerning0
You are a summer intern at ACME shipping company, which owns a harbor with cargo ships, cranes, roads, robots, parking lots, and storage piles for cargo containers. They ask you to write software that automatically generates plans, which will be executed by the robots, for moving the containers from the cargo ships to their designated storage locations. This situation can be modeled formally in Planning Domain Definition Language (PDDL) as: \
\pard\tx220\tx720\pardeftab720\li720\fi-720\sl340\sa293\partightenfactor0
\ls1\ilvl0\cf0 \kerning1\expnd0\expndtw0 {\listtext	\'95	}\expnd0\expndtw0\kerning0
A set of locations \{l0, l1, l2 ... \} that correspond to docked ships, storage areas, and parking areas. \
\ls1\ilvl0\kerning1\expnd0\expndtw0 {\listtext	\'95	}\expnd0\expndtw0\kerning0
A set of robots \{r0, r1, ... \} \
\ls1\ilvl0\kerning1\expnd0\expndtw0 {\listtext	\'95	}\expnd0\expndtw0\kerning0
A set of cranes \{k0, k1, ... \} \
\ls1\ilvl0\kerning1\expnd0\expndtw0 {\listtext	\'95	}\expnd0\expndtw0\kerning0
A set of piles \{p0, p1 ...\} \
\ls1\ilvl0\kerning1\expnd0\expndtw0 {\listtext	\'95	}\expnd0\expndtw0\kerning0
A set of containers \{c0, c1, ... \} \
\ls1\ilvl0\kerning1\expnd0\expndtw0 {\listtext	\'95	}\expnd0\expndtw0\kerning0
A symbol G that denotes the ground, on which all piles rest. \uc0\u8232 The topology of the harbor, which cannot be modified, is specified using three predicates: \'95 adjacent(l, l\'92) : location l\'92 is adjacent to location l \u8232 \'96 Note: this only specifies that there exists a one-way road from l to l\'92. To specify a two-way road, we must also add adjacent(l\'92, l) to the list of initial conditions. \u8232 \'95 attached(p, l) : Pile p is attached to location l 1 \
\pard\pardeftab720\sl340\sa240\partightenfactor0
\cf0 	\'95 belong(k, l) : Crane k belongs to location l\uc0\u8232 \
The arrangement of trucks and shipping containers, which can change over time, is specified by the following:\uc0\u8232 	\'95 occupied(l) : Location l is occupied by a robot\u8232 	\'95 free(l) : Location l is not occupied by any robot\u8232 	\'95 at(r, l) : Robot r is at location l\u8232 	\'95 loaded(r, c) : Robot r is loaded with container c\u8232 	\'95 unloaded(r) : Robot r is not loaded with any container\u8232 	\'95 holding(k,c) : Crane k is holding container c\u8232 	\'95 empty(k) : Crane k is not holding a container\u8232 	\'95 in(c, p) : Container c is in pile p\u8232 	\'95 on(c, c\'92) : Container c is on top of c\'92, which is either a container or G\u8232 	\'95 top(c, p) : Container c sits on top of pile p. If pile p is empty, we write top(G, p) \
Note that these predicates are not independent. For example, unloaded(t) can only be true if there is no c such that loaded(t,c). \
Next we specify the five possible actions: move, load, unload, put, take. \
	move(r, l, m) # move robot r from location l to location m precond: adjacent(l, m), at(r, l), free(m)\uc0\u8232 	add: at(r, m), occupied(m), free(l)\u8232 	delete: occupied(l), at(r,l), free(m) \
	load(k, l, c, r) # crane k at location l loads container c onto robot r precond: belong(k,l), holding(k,c), at(r,l), unloaded(r)\uc0\u8232 	add: empty(k), loaded(r,c)\u8232 	delete: holding(k,c), unloaded(r) \
	unload(k,l,c,r) # crane k at location l takes container c from robot r precond: belong(k,l), at(r,l), loaded(r,c), empty(k)\uc0\u8232 	add: holding(k,c), unloaded(r)\u8232 	delete: empty(k), loaded(r,c) \
	put(k,l,c,d,p) # crane k at location l puts c onto d in pile p precond: belong(k,l), attached(p,l), holding(k,c), top(d,p) add: empty(k), in(c,p), top(c,p), on(c,d)\uc0\u8232 	delete: holding(k,c), top(d,p) \
	take(k,l,c,d,p) #crane k at location l takes c off of d in pile p precond: belong(k,l), attached(p,l), empty(k), top(c,p), on(c,d) add: holding(k,c), top(d,p)\uc0\u8232 	delete: empty(k), in(c,p), top(c,p), on(c,d)  \
\pard\tx720\tx1440\tx2160\tx2880\tx3600\tx4320\tx5040\tx5760\tx6480\tx7200\tx7920\tx8640\pardirnatural\partightenfactor0
\cf0 \kerning1\expnd0\expndtw0 \
}