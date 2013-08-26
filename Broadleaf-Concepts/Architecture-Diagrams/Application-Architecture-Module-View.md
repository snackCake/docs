If you've followed the [[Getting Started]] documentation, then you'll notice that the starter project contains three Maven projects (site, core, and admin).     

This structure is recommended for enterprise Broadleaf Applications but it is not required.   For hobbyists or those building applications for small companies, they may choose to combine these three projects into a single project.

|Module Name | Description|
|--------------------|-----------------------------------------|
|site|Represents a project that produces the WAR (web archive) for a typical eCommerce website.|
|admin|Represents a project that produces the WAR for the Broadleaf Commerce admin.|
|core|Represents a project where code that is common to both the admin and the site can be maintained.|


