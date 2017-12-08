# Context

Fracreg is, in most ways, a standard MRS registry. It runs in the same kinds of
environments as other registries, it uses the same core technologies, and it
generally serves the same purpose. Fracreg was developed by a team at the
Stavanger University Hospital (SUS), and it is based to a large degree on ORPlan,
another system developed by the same team. Fracreg is ultimately intended to be
a "national registry" used across Norway, but it has a phased development plan
wherein it will first be deployed in Stavanger, then the Helse Vest region, and
finally on a national scale.

What follows is a rough sequence of development milestones. These are mostly in
order, though it's possible that prioritizations will change over time.

## Initial project goals: standalone deployment at SUS

The first goal of Fracreg project is to run in SUS only. This will allow us to
work out the details of simply deploying the system without needing to worry
about doing multiple fresh deployments concurrently.

The initial version of the registry will work in a "standalone" mode where we
minimize its interactions with other systems. This, again, is in an attempt to
avoid dealing with too many complexities at once.

## Integration with patient journals

One of the next goals for the project is to allow Fracreg to share data with
electronic patient journal (EPJ) systems like DIPS. In order to minimize
duplicate data entry, we need to be able to both send data to and receive data
from these systems. So, for example, if information about an injury already
exists in an EPJ, Fracreg should be able to fetch that data so that the users
don't need to enter it a second time. Likewise, if data exists in Fracreg that
also needs to be in the EPJ, Fracreg should be able to send it there.

This kind of system integration can be potentially complex. However, this is not
a goal that's unique to Fracreg. Several other registries have similar goals,
and there are apparently some plans underway to create a fairly general solution
to this issue of data sharing. So rather than try to address this problem on our
own, we're hoping that a solution will be built for us.

## Integration with operation planning tools

Similar to EPJ integration, we also hope to integrate Fracreg with various
operation planning tools. This way information captured in Fracreg can be made
available in those systems without duplicate data entry.

In many ways this is the same problem as integration with EPJ, and we're hoping
that a generalized solution becomes available.

## Electronic Patient Reported Outcome Measures (EPROMs)

Another feature Fracreg plans to support is the ability to send various EPROMs
to patients involved with the registry. EPROMs are a powerful emerging
technology for gathering information based on user reports, and they will be a
very useful addition to Fracreg's data gathering and analysis features.

Hemit is currently developing a system to handling EPROMs and integrating them
with MRS-based systems.

## Deploying to more hospitals in Helse Vest

Once Fracreg has been operating at SUS for a while, we will deploy it at other
hospitals in the western health region. This will certainly bring a new set of
challenges (e.g. integration with new systems), so we want to be confident in
the fundamental system before doing this.

## National deployment

Finally, once we've stabilized the systems in the western hospitals we plan to
run the registry at the national level. This will likely involve a new set of
complexities, but we should have enough experient from the prior deployments to
deal with them.
