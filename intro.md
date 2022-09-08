# Developing a distributed app with Solid

Welcome! This is my journey through developing a hospitality exchange app with Solid. I talk about technical stuff of developing. Also, as much as possible, i try to include my frustrations, annoyances, insecurities, motivations, and also reasons why i do one thing and not the other.

## Intro

Solid is a relatively young project, at least from the perspective of adoption and usability. It still has a lot to desire.

But its promise is very interesting: For user: own your data. For developer: make app on top of existing social network. No need to attract new users. Network effect is here. Your app can only make it better.

## Is this suited for you?

I'll make a knowledge assumption throughout. I won't explain what i don't feel like explaining. But i'll try to provide links as much as possible.

The technology i knew before i started: [Typescript](https://www.typescriptlang.org/), [React](https://reactjs.org/), [Solid](https://solidproject.org/), [RDF](https://www.w3.org/TR/rdf11-concepts/#data-model), [Express.js](https://expressjs.com/), and i won't explain this. In other cases i'll try to describe my learning journey.

## My agenda

I want to make apps that connect people in the real world. To be together, collaborate, and change the world to the better in the process.

I've spent a significant amount of time developing an app for collaborating in the past. But it didn't have users. So ... meh ... well ...

---

I think [Solid](https://solidproject.org/) is an interesting idea. But it suffers from being young and toothless. I haven't seen a proper social network built with Solid. Most apps seem to be just apps for developers. [Assemblée Virtuelle](https://www.virtual-assembly.org/) with their [ActivityPods](https://github.com/assemblee-virtuelle/activitypods) have [built](https://lescheminsdelatransition.org/) [interesting](https://www.virtual-assembly.org/semapps-2/) [stuff](https://www.virtual-assembly.org/siti-2/), but it's [not backwards compatible with Solid Pods](https://forum.solidproject.org/t/activitypods-adding-intelligence-to-solid-pods-with-activitypub/4962/12).

I don't care for the inception ideas. I don't care for authority in Solid. I don't care for your new Specification. I want it to work.

For me, Solid is:

1. an identity provider
2. a distributed database that apps can connect to, and owners of data can specify detailed permissions for their data - who they want to share their data with

As I said, i want to develop great real-social applications. Applications that connect people in reality.

One well explored, and popular use case is Hospitality Exchange. It's pretty clear what the system should do.

So, let's focus on Hospitality Exchange for now.

## What is it supposed to do?

Imagine you travel. You're arriving to a new place. You need a place to stay, for free.

In the simplest case:

0. Sign in
0. Write a little bit about yourself
0. See a map of people who offer to host travellers
0. Open a profile of, and read about a few potential hosts, their references from other travellers, ..., get impression that you'd be reasonably safe with them
0. Contact the potential hosts
0. Wait for their reply
0. When they reply, receive a notification
0. They can host you!
0. Confirm
0. Enjoy the new real world connection, enjoy the stay
0. After the stay, write a reference for them, and establish a friendship connection

Now, you have a home, you want to host travellers in your home:

0. Sign in
0. Write a little bit about yourself
0. Write about your home: Approximate location, and say that you can host people
0. Wait for travellers to write to you
0. Somebody wrote! You received a notification. 
0. Read a message from them, read about them, check their references ... get intuition that you'd be reasonably safe with them in your home
0. Invite them
0. They confirm
0. Enjoy the new real world connection, enjoy their stay
0. After their stay, write a reference for them, and establish a friendship connection

---

In a more complex feature:

As a traveller:

0. Share your travel plans online
0. Potential hosts will contact you and you don't have to sit back

As a host:

0. Look who's travelling into your area
0. Don't wait until they write to you. Invite them!

And that's the app we want to make!

[Next: App design](app-design.md)
