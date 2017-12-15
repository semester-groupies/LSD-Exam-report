# LSD Exam Report: Group H

AMOUNT OF CHARS W. SPACES: XXXX

At the beginning of the semester we were formed as a group consisting of five roles; the product owner, the lead developer, the architect, the quality assurance, and the devops.

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

Before we tackled Hacker-News project we had to choose what  kind of software development process we should use to implement this project.


#### Waterfall method

The Waterfall development model is one of the best known software development processes. In this process developers follow specific steps. These steps are as follows:

state requirements, analyze them, design an approach for the solution, create software framework for that approach, develop code, test the code, deploy and maintain it. After each step is done, the process continues with next step and there is no turning back to previous step. Advantages of this process are that it is easy to understand and use, it is great model for projects that are smaller in scale and have all requirements well understood. Disadvantages are that is not suitable for projects where requirements are not well understood, projects that are are long and ongoing.


#### Iterative methods

In software development iterative methods are used where project is developed in small iterative sections. After each iteration it gets review by developer team and potential end users. After these reviews insights and critique are used to determine how to tackle next iteration. This approach is suitable for projects where requirements are not fully clear and also may change during the time. 


#### Our approach


### Software Architecture

The 


### Software Design

bla bla


### Software Implementation

bla bla


### Hand-over

We didn't receive an SLA from the group(group G) before the end of the run time of the project. 

In contrast to the SLA, the group had made very good documentation of their project . They have made very detailed documentation of their applications architecture and included all the credentials needed to log into the monitoring system (grafana) and have access to their Jenkins dashboard so that we could see all aspects of their build.

Whilst it was good that they had made such extensive documentation, they should probably have thought about some security issues, most notably, the fact that they have left all of their admin passwords in plain text on their github repository. As well as this they have left example queries for their database, so all the tools are there for somebody to destroy their database at will.

They also haven't made the front end for the project at all, so we haven't been able to monitor/test the functionality of this part of the project.

So in conclusion, we felt that the documentation was of a high standard, but the missing SLA meant that we haven't had a set agreement on what we should monitor on their system. 


### Service-Level Agreement

This is the SLA that we received from Group G after we had started to write this report, so too late to be of any real use for the monitoring of the system, but it follows the standards that we covered during the class on SLA's.
Contributors:  Yoana Dandarova & Manish Shrestha

This (made up) SLA with the Group H includes the following commitment from us (Group G):

 - Maximum of 20 minutes of downtime per day (if any)
 - Response time of under an hour
 - Debugging and maintenance within 24 hours of (bug) reporting

 - Access to monitoring of system.

The SLA metric, powered by Prometheus + Grafana, can be accessed from the following link:
http://188.226.163.242:4000/dashboard/db/nodeapi-monitoring?from=1510112371364&to=1510173614756&orgId=1

Log in credentials is the default. *hint* **admin x 2** *hint*

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

There was quite a lot of confusion regarding alerts, and whose responsibility it was to set them up.They thought we should be the ones to make them, but we were reluctant to have to start making alterations in their code, so the result was that we didn't get any alerts from their system. 

Early on in the monitoring we could see that they were making an incorrect return for the hanesst_id, and that this meant they weren't appearing on the graph showing the number of posts ingested. We raised an issue on github, and they fixed it within 24 hours, and result was that they appeared somewhere pretty close to the top of the results table. 


### Technical Discussion

bla bla


### Group Work Reflection & Lessons Learned

Probably the biggest lesson we learnt is to choose wisely and plan. We feel this mainly because of our Neo4j database which caused us a lot of problems


### Conclusion

bla bla


# Litterature

[https://blog.akana.com/considerations-api-requirements/](https://blog.akana.com/considerations-api-requirements/)
