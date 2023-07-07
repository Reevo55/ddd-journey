## Big Picture Event Storming

#### Intro

The process of Event Storming in this example will be artificial and is meant to be a demonstration of the technique. In a real-world scenario, the process would be more organic and would involve the participation of all stakeholders.

Firstly the stakeholders would be identified and invited to the event. Also the domain experts would be necessary for this process. For this example domain experts and stakeholders will be represented by the author of this document with the help of ChatGPT for discovering and answering the questions typically asked to the stakeholders and domain experts.

For the whiteboard I will be using Miro. The reason for this is that it is a free tool and it is easy to use. It also has a lot of templates that can be used for this process. The template that I will be using is the Event Storming template that can be found [here](https://miro.com/miroverse/event-storming/).

#### Step 1: Domain Events

This step would require asking the invited stakeholders and domain experts questions about the domain. The questions would be about the domain events. It is effective to ask everyone to write the as many domain events as they can on sticky notes. So the people are not influenced by one another and they can write down their own ideas. It is sometimes reffered to as a silent brainstorming.

In this document I will be writing my own sticky notes and asking ChatGPT for domain events ideas as domain experts and stakeholders.

The domain events that emerged are:

- Ticket cancelled
- Seat reservation cancelled
- Payment failed
- Online booking abonded
- Ticket issued
- Seat change requested
- Seat reserved
- Showtime updated
- Payment processed
- Online booking started
- Movie schedule created

![Event storming step 1](/example-project/design/imgs/event-storming-1.png "Event Storming Step 1")

#### Step 2: Refine Domain Events

In this step, the domain events emerged in previous step are examined with participants. Also the events are grouped by order of time and synonyms are unified. Furthermore, the missing events are added to the board.

One missing event that emerged is:

- Ticked notification

![Event storming step 2](/example-project/design/imgs/step-2-missing.png "Event Storming Step 2")

#### Step 3: Process modelling

During this step, users and triggers are identified. Through investigation following map emerged:

![Event storming step 3](/example-project/design/imgs/3-step.png "Event Storming Step 3")

During the process following additional events emerged:

- Showtime update notification
- Seat change request denied
- Seat change request accepted

After the changes the refined map looks like this:

![Event storming step 2- fixed](/example-project/design/imgs/2-step-fix.png "Event Storming Step 3- fixed")

#### Step 4: Software (aggregates) modelling

In this step aggregates are found. During the process following aggregates emerged:

- Seat
- Payment
- Ticket
- Schedule

![Event storming step 4](/example-project/design/imgs/step-4.png "Event Storming Step 4")
