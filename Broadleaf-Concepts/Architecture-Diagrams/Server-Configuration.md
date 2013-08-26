The performance characteristics vary greatly between commerce applications.   The following table provides general guidelines for virtualized server configurations within a Broadleaf Commerce architecture.

Broadleaf Commerce Professional Services can provide assistance for companies needing to size, load test, or tune their Broadleaf application.    You can also request the Broadleaf Commerce Scalability White Paper by emailing info@broadleafcommerce.com.

### Sample Server Configurations
|Server Type           |Per VM Configuration Notes            |
|----------------------|---------------------------------------------------------------------------------|
|Web Server            |Apache documentation should be consulted for proper web layer sizing and tuning. |                     
|                      |RAM:  2 Gigs,   Disk:  10 Gigs,   CPU:  1 Core                                   |
|                      |The biggest driver for sizing Apache in a BLC application is the number of concurrent users or API requests.  Most sites (even quite large) will be able to run with 2 Apache nodes.|
|Application Server    |The number of application servers required can vary greatly based on how the framework is used.    Sites that are able to optimize caching and have a low amount of external integration points could handle 1,000 orders per hour on a single JVM.   Request the Broadleaf Commerce Scalability White Paper for example performance numbers.|
|                      |RAM:  4 Gigs (2 for the JVM), Disk:  10 Gigs, CPU:  2 Cores |  
|Database Server       |For larger sites, the database will benefit the most from having additional memory and CPU power.    Consult your DB documentation for information on tuning and sizing.   A typical DB configuration is shown below.   Request the Broadleaf Commerce Scalability White Paper for more information on results from our lab.|
|                      |RAM:  8 Gigs, Disk:  100 Gigs (highly dependent on order volume and catalog size), CPU:  4 Cores | 