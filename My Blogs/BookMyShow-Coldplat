# BookMyShow, can you please book my show ?

Context : Coldplay announced their concert in India but announced only 2 shows in Mumbai. BookMyShow(BMS) a leading booking platform was the official ticketing partner and took up on the task of handling such a huge crowd trying to get their hands on once a lifetime opportunity for many Indians. The ticketing partner struggled to handling this scale disappointing the fans. 


I hope everyone got their tickets. Jk! I know everyone struggled, waited and gave up on their hopes to get the tickets. My roommate's sister was trying to book tickets on call with us and out of frustration  said "All engineers are useless" and we don't even work at BMS. This motivated me to write and explain what scale was BMS looking at and maybe we should just cut them some slack and maybe learn from their mistakes. This reminds me of the RCA call we used to do at my previous company where the motive was to understand what happened and how we can avoid it in the future in a blameless manner.

Before even getting started kudos to the BMS Tech, SRE Team. Taking up task like this itself is commendable in itself and I can only imagine the pressure the BMS Team went through and the critique they might be getting. I hope the leadership supports them

Lets dive in!

## What scale are we looking at ?

Total Tickets : 1,50,000

Tickets allowed per user: 4

The last queue position I saw was **~12,00,000.**

Current Mumbai Population ( 21,673,000) assuming 5% of the population is trying to book proactively at 12:00PM IST i.e 1083650. Now this is just Mumbai, since they are doing a show only in Mumbai I am sure people from all over the country and from other parts of the world as well were trying to book(just like me and my roommate) since it is in December many people visit home and want to go to the concert. But for simplification reasons lets just assume 1000000 people are trying to book. People also logged in through their phones and laptops which inturn increases the number of users trying to access. It becomes 20,00,000. 

To give you an idea an average nginx server handles 10,000 concurrent connections at max. Obviously this is an oversimplification and then they have redundant servers to server and distributed across and CDN as well to help with things. This is just to imagine the scale we are deal with

Even if we assume these 20,00,000 goes to a single server, It will take 200*2s = 400s to server all these requests. These many requests cause another set of problems that is because the server is running at max capacity it sometimes crashes or slows down processing. 

There are other issues as well with such a large scale such as DB throttling, Third-Party services crashing due to the huge load, payment Gateways may also see a sudden surge and much more. 


## What the queue ?

I am not sure if the queue system is something unique but I have seen it for the first time. I am sure many people were mainly frustrated because they had to wait for so long in the queue and eventually got nothing. I feel people would rather prefer not getting tickets right away because they know it got sold out immediately rather than waiting in the queue endlessly. 

### Why would they introduce a queue system ?

Well, If I were a developer with BMS and you told me to be ready for something like this and if I am not sure if my systems would handle it well I would do the same thing. I guess the idea was to create a bottle-neck so that the ticket booking system isnt overwhelmed and they dont reach a complete shutdown. I guess what they didn't realise is that all the services would eventually come to under duress because of the sheer number of people accessing the website concurrently. To give an example:- In on of the interview, the Hotstar Tech guy said that often their Home Page API used to crash after Dhoni used to get out of the cricket match, because people used to shut the match and were redirected to Home Page and this used to create a spike.

Also, the time taken to process queue was large because each user was alloted a max of 4 mins to book tickets. 

Honestly, I wasnt expecting anything but things like this;





## What all went wrong

### 1. User getting logged out

My educated guess would be since people were continously refreshing their home pages the user session management service might have went down or timing out and the sessions werent authenticated leading to people getting logged out.

### 2. People did not get OTPs

Now since people got logged out, naturally everyone tried to log in again and here goes the OTP service.

### 3. 505 Bad Gateway Errors
This is the worst thing that can happen in scenarios like this. Your entire service going down, although only few servers might have gone down because people were still able to access after trying to refresh so thats a good thing.

### 4. Refreshing got slowed
This is still an acceptable thing in my opinion but could have been mitigated by effective use of CDNs.

### 5. Coming Soon button not switching'
I am not sure what might have went wrong here but this is my guess. BMS might be using server side rendering and might have set the time in something like Redis as to when an event should open for booking. There is a possibility that because of some server failures the website was served through their cache or from the CDN Cache. Again this behavior was inconsistent for the users.

### 6. Queue details updating very late
I am assuming the Queue details were updated through asynchronous messaging and aggregating the update events. There might be some delay in updating the queue because of the high volume and few people complaint of getting random numbers probably due to race conditions.

### 7. Current Availability not refreshing and showing inaccurate data.
I was on a call with my friend who was trying to book it. Now the interesting thing was not everyone was seeing the same details and the numbers were not consistent. Sometimes the available tickets went up which made people think that more seats are opening up.

I think what happened there was, BMS was operating out of an eventually consistent distributed database. Database replicas weren't in sync probably due to the sheer scale and user requests were served by different servers every time, this led to the fluctuation and inaccuracy in the seats availability

### 8. People not able to book even after getting through the queue
Not sure how many people faced this but one of friend couldnt book the tickets even after getting through the queue. This again seems to be an issue with the Database not being 
consistent as users were not able to see the seats available to book.

### 9. Payment confirmation failures
Even if you make it through the payment gateway, people didnt get the confirmation from BMS right away. This was probably due to the Gateway being overwhelmed but this is still fine because these events might be asynchronous and will eventually be processed.


## What I would have done ?
(This is from whatever knowledge I have gained on system design so far from blogs, books, podcasts, case studies etc)


### Just add more servers ?

All the issues highlight above are mainly because the servers were overwhelmed. Then why not just add servers on demand or use auto-scaling EC2 instances since you are on cloud anyway ? Well its not really that simple

Before I understood how AWS EC2 Auto Scaling actually works I used to think it was easy to manage load because EC2 auto-scales. Learnt it the hard way, it doesn't and sometimes adding more compute power doesnt solve all your problems.

EC2 instances can take upto 45mins to detect and scale loads and by the time this happens you are finished. The auto-scaling is subject to things like availability of instances in the region/availability zone. AWS is not gonna end up giving all their compute power in the region just because you have a load surge. 

BMS could have warmed up their servers and auto-scaled before the ticket-sales started by putting dummy load or they could have reserved spot instances giving them the compute power they would require. 


### If its not your primary use-case, just by-pass it

In of their podcasts Hotstar was talking about how they bypass few subsystems if those services are under duress on demand. For eg:- They are prepared to bypass their Authentication system, Payment System and Recommendation system such that it doesn't block the user flow to actual USP (Cricket match)

Maybe BMS could have done something like this with their Authentication system/OTP Service


### Multi-CDN Strategy to not stress out a single CDN provider in a particular region.

Slow refreshing, switching of the "Book Now" button looks to be due to servers/CDN getting over-loaded and serving incorrect and stale response. This could have been probably avoided by introducing high redundancy and availability.


### Using a trusted highly consistent and available cloud-native database.
Well if I have to pull off a task like that, I would go with a managed service like Amazon DynamoDB. It has proved the test of time and is known to give consistently performance even at a massive scale. Amazon itself relies on it for their PrimeDay sales and other critical operation.


### Working with partners
Working with your third-part partners is very important because if you are under a surge they are too. Getting in touch with them and ensuring their preparedness be it AWS, CDNs, Payment Gateways is very crucial because if they fail you are gonna fail too.

### Load Testing the application as a whole and not just the service.
Users interact with the application and not the service. Hence, it is crucial to test the application as a whole. For example. assuming BMS might have tested and tweaked the queue outflow with the booking system but to get the queue system they need to login and get the OTP and get through the home page and the "Book Now" button. Thus, all the services should be resilient enough for that scale.
