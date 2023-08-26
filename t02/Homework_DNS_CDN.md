---
tags: [dns, cnd, devops11, homework] 
---

## Question
### Draw a infrastructure architecture diagram

Draw a infrastructure architecture diagram for the hands-on that you just did for CDN and DNS. It should include 3 components:

1. the jiangren.com.au website
2. the CloudFront CDN component
3. the Route53 hosted zone

Please reflect following in the diagram:

1. the relationship between above components
2. the traffic flow when a user requests your "your_name.jiangrendevops.com" website. i.e. what happens after a user type in "your_name.jiangrendevops.com" website in a browser?

You can use [draw.io](https://app.diagrams.net/) to draw this diagram easily. Make sure you save both XML file(.drawio) and the diagram picture file (.png) when you finish, so that you can edit your diagram again in the future.
(source from 
https://github.com/JiangRenDevOps/DevOpsNotes/blob/master/WK1_CDN_DNS/homework.md)

## Answer
### Diagram

```
User (Browser) ---> Route53 (DNS Service) ---> CloudFront (CDN)
   ^                                               |
   |                                               |
   +-----------------------------------------------+
                                (if necessary)
                                jiangren.com.au (Origin Server)
```

### Explain
Let's break down the diagram and explain each step in a simple way:

1. **User (Browser)**: This is the person visiting your website. They type in your website's URL (in this case, "your_name.jiangrendevops.com") into their web browser.

2. **Route 53 (DNS Service)**: DNS stands for Domain Name System, and it's essentially the phone book of the internet. When the user types in your website's URL, that request goes to Route 53. Route 53 looks up the URL and finds out where the website's content is stored. In this case, it's stored on a CDN network (CloudFront).

3. **CloudFront (CDN)**: CDN stands for Content Delivery Network. This is a network of servers that store copies of your website's content in different locations around the world to deliver it faster to users. When Route 53 directs the user's request to CloudFront, the CDN checks if it has a recent copy of the requested content. If it does, it sends that content directly to the user's browser.

4. **jiangren.com.au (Origin Server)**: This is where the original, master copy of your website is stored. If CloudFront doesn't have a recent copy of the requested content, it goes back to the origin server to fetch the latest version. After fetching the content, CloudFront delivers it to the user and also stores a copy for future requests.

5. **Back to User**: Regardless of whether the content is served directly from the CDN or after a round trip to the origin server, the final content is delivered back to the user's browser for them to view.

The main goal of this setup is to make your website load faster for users. By storing copies of your website at different locations closer to users (CDN), and by using a smart lookup system (DNS), you can serve your website content more quickly and efficiently.
