 SNLP: Systematic Nonlinear Planner 
The tests and outputs are for my implementation of SNLP for the following problem. The tests are finished in milliseconds. 

You are a summer intern at ACME shipping company, which owns a harbor with cargo ships, cranes, roads, robots, parking lots, and storage piles for cargo containers. They ask you to write software that automatically generates plans, which will be executed by the robots, for moving the containers from the cargo ships to their designated storage locations. This situation can be modeled formally in Planning Domain Definition Language (PDDL) as:

	• A set of locations {l0, l1, l2 ... } that correspond to docked ships, storage areas, and parking areas.
	• A set of robots {r0, r1, ... }
	• A set of cranes {k0, k1, ... }
	• A set of piles {p0, p1 ...}
	• A set of containers {c0, c1, ... }
	• A symbol G that denotes the ground, on which all piles rest.

The topology of the harbor, which cannot be modified, is specified using three predicates: • adjacent(l, l’) : location l’ is adjacent to location l

– Note: this only specifies that there exists a one-way road from l to l’. To specify a two-way road, we must also add adjacent(l’, l) to the list of initial conditions.

	• attached(p, l) : Pile p is attached to location l 1￼
	• belong(k, l) : Crane k belongs to location l

The arrangement of trucks and shipping containers, which can change over time, is specified by the
following:
	• occupied(l) : Location l is occupied by a robot
	• free(l) : Location l is not occupied by any robot
	• at(r, l) : Robot r is at location l
	• loaded(r, c) : Robot r is loaded with container c
	• unloaded(r) : Robot r is not loaded with any container
	• holding(k,c) : Crane k is holding container c
	• empty(k) : Crane k is not holding a container
	• in(c, p) : Container c is in pile p
	• on(c, c’) : Container c is on top of c’, which is either a container or G
	• top(c, p) : Container c sits on top of pile p. If pile p is empty, we write top(G, p)

Note that these predicates are not independent. For example, unloaded(t) can only be true if there is no c such that loaded(t,c).

Next we specify the five possible actions: move, load, unload, put, take.
	move(r, l, m) # move robot r from location l to location m precond: adjacent(l, m), at(r, l), free(m)
	add: at(r, m), occupied(m), free(l)
	delete: occupied(l), at(r,l), free(m)
	load(k, l, c, r) # crane k at location l loads container c onto robot r precond: belong(k,l), holding(k,c), at(r,l), unloaded(r)
	add: empty(k), loaded(r,c)
	delete: holding(k,c), unloaded(r)
	unload(k,l,c,r) # crane k at location l takes container c from robot r precond: belong(k,l), at(r,l), loaded(r,c), empty(k)
	add: holding(k,c), unloaded(r)
	delete: empty(k), loaded(r,c)
	put(k,l,c,d,p) # crane k at location l puts c onto d in pile p precond: belong(k,l), attached(p,l), holding(k,c), top(d,p) 
	add: empty(k), in(c,p), top(c,p), on(c,d)
	delete: holding(k,c), top(d,p)
	take(k,l,c,d,p) #crane k at location l takes c off of d in pile p precond: belong(k,l), attached(p,l), empty(k), top(c,p), on(c,d) add: holding(k,c), top(d,p)
	delete: empty(k), in(c,p), top(c,p), on(c,d)
