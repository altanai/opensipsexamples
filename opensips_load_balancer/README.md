## Opensips as Load Balancer 

traffic routing based on load and avaiable slot of destinations
Opensips is aware of the capacity of each destination as it is preconfigured with the maximum load accepted by the destinations
aware of the load status (as number of ongoing calls) of each destination 
will share traffic accordingly with less loaded destinations 
feedback from gdestination to update routing rules


Ref : https://www.opensips.org/Documentation/Tutorials-LoadBalancing