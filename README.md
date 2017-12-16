
# LSD Exam Report: Group H

AMOUNT OF CHARS W. SPACES: 17,419

At the beginning of the semester we were formed as a group consisting of five roles; the product owner, the lead developer, the architect, the quality assurance, and the devOps.

These five roles were supposed to make the team that were to create a clone version of the Hacker News website. With the information given about the assignment it was clear that we should recreate a functional version of both the back- and front end of the Hacker News site with as many features as possible.

Later in the process we were supposed to hand over our back end for monitoring by another group, there were to create github issues on their findings and we ought to then fix these bugs found.

This is the report on the process from locating and analyzing the requirements to the final reflections on the whole idea, concept, and challenges we faced throughout the project assignment.


### System Requirements

In this section we will dig into the functional requirements of the Hacker News clone site. The site is focused on non-mainstream social news in the form of links to blogs or articles found on the internet. Guests on the site should be able to read these posted stories, and should also be able to register on the site in order to login to create stories, comments, or comments on comments themselves. Moreover all logged in users should be able to update their profile.

Any logged in user should be able to assign points to stories and comments, and should also be able to receive points for their own profile; so-called karma points. It should also be possible to flag content somehow as spam as well as reporting users or content of violation of the site rules and guidelines.

In addition to these aforementioned requirements, the Hacker News clone should be able to accept incoming traffic / behaviour from a simulator acting as users on the site. The site should accommodate requests sent to the following URLs:



*   /post
    *   This should accept incoming post requests which will contain a JSON in the request body. The JSON will contain information of either a story, comment, poll, or pollopt.
*   /latest
    *   This should accept incoming get requests and should return an integer with the latest hanesst id stored in the Hacker News clone database.
*   /status
    *   This should accept incoming get requests and should return a string representing the state of the back end, e.g., "Alive", "Update", and "Down".

Besides these functional requirements, there are also a number of non-functional ones. The Hacker News clone must be accessible at least 95 % of the time. It has to look like the existing Hacker News and must react whenever a story or comment is "flagged". It should perform some kind of buffering for incoming stories and comments so that in the case of maintenance or alike, the data won't get lost.

All functions and components should be tested to ensure functionality as well as automating tests and providing the project with code coverage statistics will provide reliability to the product.

Stories must be able to other users once it's created, and all stories or comments are subject to be processed and cross checked for forbidden words, and it shouldn't take too long time for the system to complete. The system itself will not be browser dependent, so in other words it should be able to run on any browser with an active internet connection.

There should be mockups for the front end, paper prototypes because they're easy to change to new looks. Lastly, the Hacker News clone will have requirements to its users like rules of conduct, and the Hacker News clone does not support any kind of instances of Copyright Infringement, and in any case the story, comment, or alike will be deleted, and the user IP will be banned from the server.


### Development Process

Before we started working on Hacker-News project we had to choose what kind of software development process we should use to implement this project. From the beginning we knew all the specified requirements so we started by creating use cases. We knew that implementing hacker news project with waterfall method wouldn't be an option for us, as the  waterfall method is more suitable for projects that are smaller in scale and not meant to change in the requirements. We decided to develop the project incrementally with iterative development methods, slicing use cases into smaller use stories and implementing them iteratively. We were not strict on working using agile methodology by doing daily standup meetings, sprint planning sessions or keeping a backlog. 

After implementing Jenkins as our continuous integration tool we could deliver our code to production in more rapid and incremental pace. After a certain user story was ready it was released to production with help of Jenkins CI, after that the developer could continue with the next user story. Although jenkins CI is not a part of agile development methodology it was an important part of our iterative development process.


### Software Architecture

The initial design is a bit different from the actual design that ended in production. The initial design had the database and backend in the same droplet to enable better communication between them. This ended up being an expensive solution since our database (Neo4j) require a lot of resources. The next step was to move the database to a different provider and move the frontend and backend in the same droplet (due to economic reasons).

The architecture for the system is split to the minimum amount of virtual machines as possible.

 The idea is to have the systems as close as possible so that they can communicate the fastest possible way and make all processes faster. The frontend and backend exist in the same machine and the database on its own. This allows for database scalability independent of the frontend and backend.  So we ended up with 3 virtual machines. One was equipped with Jenkins to handle git commits, testing and building the new versions of the frontend and backend, one to host our database and finally one for the frontend and backend. 

Although this configuration reduced the cost of operating the system it made the frontend and the backend compete for resources when the simulator started hitting the backend with 250 requests per minute and the frontend also putting stress on the backend with expensive requests. This problem appeared as the requests started increasing. Also this setup made it really difficult for us to implement a load balancer with Docker swarm on the system because of the lack of resources. Later two more virtual machines were spawned to host the ELK stack and the monitoring system with Grafana and Prometheus. 

Also it is also worth mentioning that the technologies used to bring the project to fruition changed as we figured that the ones we selected in the beginning had some drawbacks. The initial plan was to use node js with handlebars as the templating engine  to serve precompiled paged and express to handle the routes. This would allow for swarming but at that time and point we had no knowledge of that option, and our problem was that we needed to keep them separately in order to be able to monitor them, find  bottlenecks and scale independent of each other. Next a discussion was made on what we should we use and we chose node js with express for the API (backend) and Angular 4 for the frontend even though the team did not have knowledge of the technology but it was clear that it is very good option to create websites with refreshing items in the DOM and it was interesting to the members of the group. 


### Software Design

From the start of the project we were certain that we would use node js as our backend framework. It is a framework we all like to work with, and using the Express module, it's really good for creating http servers and rest APIs.For our front end we have normally been using Angularjs, so this time we decided to try and create the front end using Angular 4, the newest version. 

As we have plenty of experience in making these web applications our main concern for this project was the database. We had decided from an early stage to use Neo4j as it has the ability to make very deep relationships between nodes, so it would be ideal for the relationships between posts and comments. Despite finding out fairly early on that the hacker news website doesn't use a graph database, we decided to stick with Neo4j for our implementation, and see if we could make it work for us.


### Software Implementation

We will cover how we implemented our solution in three parts:

**Front End:**

We created a front end using Angular 4.0 that has the look and feel of the original hacker news website. We started by coding the minimum functionality to make the website function,

as described in the project requirements, so a user can create an account, login and post stories, as well as being able to view the feed of previously posted stories. This front end communicates with the backend server via api endpoints.

**Back End:**

For the backend we created a node js express server to communicate with  the front end. This would mean coding two sets of api calls, one for 'normal' users and another for the simulator. The reasons we made two sets of calls are that the requests from the simulator contain the hanesst_id, and normal requests do not. Also, to enable the simulator to post more quickly to the api, we allow it to post without having to log a user on. We check that the user exists, then accept the post. This cuts down the time taken to decrypt the passwords (we are using bcrypt, a slow algorithm, for encryption of passwords).

**Database:**

As we mentioned in the earlier section, we decided to use Neo4j for our database. We thought that it would offer the best solution for linking stories to comments, and comment on comments etc. After some experimenting we found some queries that did what we wanted, and thought that they would work just fine. As the simulator started to make posts to the api it seemed that the database was working well, but as the traffic increased and the size of the dataset grew, we found that our database couldn't cope. We have spent countless hours trying to streamline the queries for fetching posts, as well as indexing on ids, increasing the power of our droplet and reducing the checks on incoming data, but we only managed to make a very small difference. This resulted in us only consuming around a third of the data from the simulator. As we don't have the funds to use the enterprise version of Neo4j, we have very limited options for load balancing/ scaling of our database


### Hand-over

We didn't receive an SLA from the group(group G) before the end of the run time of the project. 

In contrast to the SLA, the group had made very good documentation of their project . They have made very detailed documentation of their applications architecture and included all the credentials needed to log into the monitoring system (grafana) and have access to their Jenkins dashboard so that we could see all aspects of their build.

Whilst it was good that they had made such extensive documentation, they should probably have thought about some security issues, most notably, the fact that they have left all of their admin passwords in plain text on their github repository. As well as this they have left example queries for their database, so all the tools are there for somebody to destroy their database at will.

They also haven't made the front end for the project at all, so we haven't been able to monitor/test the functionality of this part of the project.

So in conclusion, we felt that the documentation was of a high standard, but the missing SLA meant that we haven't had a set agreement on what we should monitor on their system. 


### Service-Level Agreement

_This is the SLA that we received from Group G after we had started to write this report, so too late to be of any real use for the monitoring of the system, but it follows the standards that we covered during the class on SLA's._
Contributors:  Yoana Dandarova & Manish Shrestha

This (made up) SLA with the Group H includes the following commitment from us (Group G):

 - Maximum of 20 minutes of downtime per day (if any)
 - Response time of under an hour
 - Debugging and maintenance within 24 hours of (bug) reporting

 - Access to monitoring of system.

The SLA metric, powered by Prometheus + Grafana, can be accessed from the following link:
http://188.226.163.242:4000/dashboard/db/nodeapi-monitoring?from=1510112371364&to=1510173614756&orgId=1

Login credentials is the default. *hint* **admin x 2** *hint*

Grafana linked to the project portrays 4 graphs:

 1. Up time of the system
 2. Total Active Requests
 3. Total CPU process usage
 4. HTTP request duration (in ms)

Links to Prometheus and Grafana:

http://207.154.245.251:9000/

http://207.154.245.251:4000/


### Maintenance and Reliability

Without actually having an SLA from the group, we have tried to monitor the system under the same criteria that we had outlined in our SLA, as well as checking that it meets the requirements outlined in the project description .

As stated above, accessing the monitoring dashboard and metrics for the project was very easy. They have a very high uptime , 100 % , this being well above the 95% requirement for set out in the project description, and their system always reports 'Alive'. 

There was quite a lot of confusion regarding alerts, and whose responsibility it was to set them up.The group we whos system we were operating  thought we should be the ones to make them, but we were reluctant to have to start making alterations in their code, so the result was that we didn't get any alerts from their system. As an example, on our own system we have set up alerts from grafana to slack so that we are informed whenever the system goes down. 

Early on in the monitoring we could see that they were making an incorrect return for the hanesst_id, and that this meant they weren't appearing on the graph showing the number of posts ingested. We raised an issue on github, and they fixed it within 24 hours, and result was that they appeared somewhere pretty close to the top of the results table. 


### Technical Discussion

We seen this project as a great "fast learner", however we felt lack of time when we had to implement new technologies, like grafana, Prometheus and ELK stack. Even though we did them, we just did not have enough time to explore them deep enough, very often we just scratched the surface in order to do what we needed to do. We did not feel comfortable because of this, since there were a lot of tools which we would definitely like to explore more in depth. 


### Group Work Reflection & Lessons Learned

Even though we see this project little bit big for such a little time, we have definitely learnt a lot of new things which are amazingly helpful e.g. our CI chain which basically minimises the risk of corrupting the master branch by human factor as well as preventing its deploy which could result in fatal failures in the live systems. Another one which we would like to mention is Docker. Using this container based technology we were able to save enormous amount of time during deployment since it wraps whole application together with its dependencies as well.

Another thing we would like to mention is our database setup. We chose Neo4j, but now we can see that it was not the best option. If we knew from the beginning what kind of problems we will face, we would probably use combination of two databases. That is SQL for stories itself and Neo4j for for more relationship-oriented data such as comments and nested comments. With this setup we would be able to display stories on frontend lightning fast and then we could load comments for concrete post. 

The roles played a significant role in the beginning of the development creating the backlog and estimating the stories. This gave a feeling of virtual deadlines that we had to follow and report to each other, evaluate the progress and keep the implementations on track. It gave a good structure developing the product though towards the end we kinda drifted from these roles assigned to each other. Probably because everybody wanted to be involved in different parts of project development and the deadline starting to close in. The choice of technologies was made by discussing what the group members were comfortable with, or wanted to experiment with. Overall the roles gave some structure to group allocating responsibilities but if we devoted some more time keeping the roles it would have been more constructive. 


### Conclusion

We feel that we gained a lot from working on this project. We have made plenty of web applications, so that wasn't anything new. However, we during the project we have learnt how to build a continuous integration / continuous deployment chain that we can build and deploy our code automatically, with no human intervention needed. We have also learnt how cool it is to have monitoring of our system, using grafana and prometheus, and alerts to tell us when things are going wrong. Also, we have gained more experience and knowledge of using docker.

These are all things that we know we will use when we finish studying, so we feel that we have had a very positive outcome from this project. 


# Literature

[https://blog.akana.com/considerations-api-requirements/](https://blog.akana.com/considerations-api-requirements/)

[https://angular.io/docs](https://angular.io/docs)/

[https://neo4j.com/docs/operations-manual/current/installation/requirements/](https://neo4j.com/docs/operations-manual/current/installation/requirements/)
