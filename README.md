# discourse-autoscaled
simple discourse AWS deployment with autoscaled web containers

---

There are several posts on meta.discourse.org that talk about using AWS, but I haven't been able to find simple instructions to setup autoscaling with Discourse. One of the more interesting posts on the subject was:

> Static-machine-per-AZ is trickier than one machine for everything (separate database/redis, need to transport containers around on rebuild, etc), but that’s just the cost of doing business in AWS, where there are no guarantees about the longevity of anything running in a single AZ. Autoscaling, on the other hand, requires automating a whoooooooole pile of stuff (like central container build, pulling down the container image from somewhere, ensuring the app.yml is available, dealing with in-progress rebuilds, and a bunch of other stuff), and then on top of that, automating it quickly enough that the autoscaling is actually useful (no point taking 20 minutes to do all that stuff because by then the load spike’s probably gone because everyone’s left because your site was down), so you’re probably going to have to automate the build of the entire AMI on rebuild, which is another layer of entertaining complexity. Don’t forget the fiddling required to gracefully transition between launch configs when you roll your new AMI – and somewhere in there you need to run database migrations…

If you're using AWS and paying for all of your capacity all the time, you're really paying a high price.
I paid much less for high spec dedidcated hardware.
So the point of AWS is using small containers and autoscaling to make sure you're using only what you need.

Your Redis (ElasticCache) and Postgres (RDS) backend sizes could also be dynamically scaled up and down, but that will be a seperate issue.

# Where to start?

WordPress?

https://www.plesk.com/blog/product-technology/autoscaling-wordpress-with-docker-aws/

https://cloudonaut.io/wordpress-on-aws-you-are-holding-it-wrong/

https://github.com/widdix/aws-cf-templates/tree/v2.0.1/wordpress

- Options:

Kubernetes
Ansible
CloudFormation (AWS only)
Packer (all providers)



# How big to scale it?

* [particularly absolute peak page views per second](https://meta.discourse.org/t/general-guidelines-for-size-of-aws-instance-types/85953/2)
* [what percent of your pageviews are anonymous versus logged in?](https://meta.discourse.org/t/general-guidelines-for-size-of-aws-instance-types/85953/6)

> According to Amazon you lose 1% of sales with every extra 100ms load time. Today, customers expect your page to load in less than 3s – in their browser – not on your server.

# Making it run:

* Postgresql Database: RDS
* Redis: ElasticCache
* Centralized Container Build:

# Making it last:

* Bumping version numbers
* Dealing with "in-progress" rebuilds: (What if you were trying to "scale up" during your spike?
* Timing / implementing Database Migrations
* How to gracefully transition between launched configs?

# References

* https://github.com/ento/discourse_aws
> Rails + Redis: Docker container on Elastic Beanstalk Single Instance deployment
* https://medium.freecodecamp.org/how-to-set-up-an-internal-team-forum-in-half-a-day-using-discourse-b13588d907fe 
> It’s possible to run multiple replicas for Discourse web-server for scaling up. It should work, but I haven’t tried yet.
* https://web.archive.org/web/20140424075917/http://0ak.org/how-to-setup-discourse-on-amazon-aws/
* http://stroupaloop.com/blog/discourse-setup-using-aws/

Relevant threads:
* RDS, 
