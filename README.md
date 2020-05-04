# This contains the builder metadata for the Paketo Buildpacks builders. 

**Note:** Read through this [link](https://buildpacks.io/docs/operator-guide/create-a-builder/) for more information about creating a custom builder.

The following builders are supported:  
**Tiny**: `gcr.io/paketo-buildpacks/builder:tiny`  
**Base**: `gcr.io/paketo-buildpacks/builder:base`  
**Full**: `gcr.io/paketo-buildpacks/builder:full-cf`  

### Using a builder for a specific Platform API version
We maintain Paketo builders for the latest, n-1, and n-2 Platform API versions. 

These are published to the following paths:   
**Latest**: `gcr.io/paketo-buildpacks/builder:<BUILDER>-platform-api-latest`  
**n-1**: `gcr.io/paketo-buildpacks/builder:<BUILDER>-platform-api-n-1`  
**n-2**: `gcr.io/paketo-buildpacks/builder:<BUILDER>-platform-api-n-2`  
**\<API Version\>**: `gcr.io/paketo-buildpacks/builder:<BUILDER>-platform-api-<API Version>`  

### Using specific versions of a builder
Paketo builders are published with a version tag. Each builder also has a tag with both the builder and Platform API versions.

**\<Version\>**: `gcr.io/paketo-buildpacks/builder:<Version>-<BUILDER>`
**\<Version\> with \<API Version\>**: `gcr.io/paketo-buildpacks/builder:<Version>-<BUILDER>-platform-api-<API Version>`
