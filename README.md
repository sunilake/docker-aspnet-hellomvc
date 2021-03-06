<figure>
<img src="http://www.hypergrid.com/wp-content/themes/hypergrid/img/logo.png" alt="" />
</figure>

This project was cloned from the official Microsoft repository: **<https://github.com/aspnet/Home/tree/dev/samples/1.0.0-rc1-final/HelloMvc>**

To run & manage this simple Docker ASP.NET "Hello MVC" application on 18 different clouds and virtualization platforms (including vSphere, OpenStack, AWS, Rackspace, Microsoft Azure, Google Compute Engine, DigitalOcean, IBM SoftLayer, etc.), make sure that you either:
-   **Sign Up for HyperForm SaaS** -- <http://dchq.io>, or
-   **Download HyperForm On-Premise Standard Edition for free** -- <http://dchq.co/hyperform-on-premise.html>

[![Customize and Run](https://dl.dropboxusercontent.com/u/4090128/dchq-customize-and-run.png)](https://www.dchq.io/landing/products.html#/library?org=DCHQ)


Customize & Run all the published Docker ASP.NET application templates and many other templates (including multi-tier Java application stacks, Python, Ruby, PHP, Mongo Replica Set Cluster, Drupal, Wordpress, MEAN.JS, etc.)

### ASP.NET Hello MVC

[![Customize and Run](https://dl.dropboxusercontent.com/u/4090128/dchq-customize-and-run.png)](https://www.dchq.io/landing/products.html#/library?org=DCHQ&bl=2c9180875663d1ca01567b661cb14110)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
aspnet-mvc:
  image: dchq/aspnet-hellomvc:latest
  mem_min: 50m
  host: host1
  publish_all: true
  cluster_size: 1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


### Nginx and ASP.NET MVC

[![Customize and Run](https://dl.dropboxusercontent.com/u/4090128/dchq-customize-and-run.png)](https://www.dchq.io/landing/products.html#/library?org=DCHQ&bl=2c9180875663d1ca01567b66de00411f)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
nginx:
  image: nginx:latest
  publish_all: true
  host: host1
  mem_min: 50m
  plugins:
    - !plugin
      id: 0H1Nk
      restart: true
      lifecycle: on_create, post_scale_out:aspnet-mvc, post_scale_in:aspnet-mvc
      arguments:
        # Use container_private_ip if you're using Docker networking
        - servers=server {{aspnet-mvc | container_private_ip}}:5004;
        # Use container_hostname if you're using Weave networking
        #- servers=server {{aspnet-mvc | container_hostname}}:5004;
aspnet-mvc:
  image: dchq/aspnet-hellomvc:latest
  mem_min: 100m
  host: host1
  publish_all: false
  cluster_size: 1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Apache HTTP Server and ASP.NET MVC

[![Customize and Run](https://dl.dropboxusercontent.com/u/4090128/dchq-customize-and-run.png)](https://www.dchq.io/landing/products.html#/library?org=DCHQ&bl=2c9180865663cf2701567b67b9fc5dc1)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
http-lb:
  image: httpd:latest
  publish_all: true
  mem_min: 50m
  host: host1
  plugins:
    - !plugin
      id: uazUi
      restart: true
      lifecycle: on_create, post_scale_out:aspnet-mvc, post_scale_in:aspnet-mvc
      arguments:
        # Use container_private_ip if you're using Docker networking
        - BalancerMembers=BalancerMember http://{{aspnet-mvc | container_private_ip}}:5004
        # Use container_hostname if you're using Weave networking
        #- BalancerMembers=BalancerMember http://{{aspnet-mvc | container_hostname}}:5004
aspnet-mvc:
  image: dchq/aspnet-hellomvc:latest
  mem_min: 100m
  host: host1
  publish_all: false
  cluster_size: 1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

