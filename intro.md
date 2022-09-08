# Developing a distributed app with Solid

Welcome! This is my journey through developing a hospitality exchange app with Solid. I talk about technical stuff of developing. Also, as much as possible, i try to include my frustrations, annoyances, insecurities, motivations, and also reasons why i do one thing and not the other.

## Intro

Solid is a relatively young project, at least from the perspective of adoption and usability. It still has a lot to desire.

But its promise is very interesting: For a user: own your data. For a developer: make app on top of existing social network. No need to attract new users. Network effect is here. Your app can only make it better.

## Is this suited for you?

I'll make a knowledge assumption throughout. I won't explain what i don't feel like explaining. But i'll try to provide links as much as possible.

The technology i knew before i started: [Typescript](https://www.typescriptlang.org/), [React](https://reactjs.org/), [Solid](https://solidproject.org/), [RDF](https://www.w3.org/TR/rdf11-concepts/#data-model), [Express.js](https://expressjs.com/), and i won't explain this. In other cases i'll try to describe my learning journey.

## My agenda

I want to make apps that connect people in the real world. To be together, collaborate, and change the world to the better in the process.

I've spent a significant amount of time developing an app for collaborating in the past. But it didn't have users. So ... meh ... well ...

---

I think [Solid](https://solidproject.org/) is an interesting idea. But it suffers from being young and toothless. I haven't seen a proper social network built with Solid. Most apps seem to be just apps for developers. [Assemblée Virtuelle](https://www.virtual-assembly.org/) with their [ActivityPods](https://github.com/assemblee-virtuelle/activitypods) have [built](https://lescheminsdelatransition.org/) [interesting](https://www.virtual-assembly.org/semapps-2/) [stuff](https://www.virtual-assembly.org/siti-2/), but it's [not backwards compatible with Solid Pods](https://forum.solidproject.org/t/activitypods-adding-intelligence-to-solid-pods-with-activitypub/4962/12).

I don't care for the inception ideas. I don't care for authority in Solid. I don't care for your new Specification. I want it to work.

[comment]: <> (I don't care for your new Specification)
[comment]: <> (it's unclear to me who is "your" here. Maybe adding a link under "Specialization" will help, or changing "your" to somthing more specific?)

For me, Solid is:

1. an identity provider
2. a distributed database that apps can connect to, and owners of data can specify detailed permissions for their data - who they want to share their data with

As I said, i want to develop great real-social applications. Applications that connect people in reality.

One well explored, and popular use case is Hospitality Exchange. It's pretty clear what the system should do.

[comment]: <> (would be great if you mentioned somewhere here, here-here or elswhere on this page, how your collaboration with me or/and OHN started, what OHN is about and how it fits together with Your agenda. Maybe also a few words about sleepy.bike, who is it for and where it comes from? I'll be happy to discuss or draft parts of this if it was helpful.)

[comment]: <> (I would describe OHN as an effort to design and build next generation hospitality exchange platform. One that would be federated (decentralized and interconnected\) in order to address issues and the long-term consequances many hospex communities have faced over the years of existance and which we believe are related to centralized infrastructure they've been built on.
Or something along these lines.)

So, let's focus on Hospitality Exchange for now.

## What is it supposed to do?

Imagine you travel. You're arriving to a new place. You need a place to stay, for free.

In the simplest case:

0. Sign in
0. Write a little bit about yourself
0. See a map of people who would like to host fellow travellers

[comment]: <> (I like also a phrasing "people who invite fellow travellers to stay at theirs home/place". For me "people who offer to host sb" seems somehow very transactional, like the verb "to offer" implided that ones gets something in return. And I know usually they do, but I feel like it's better not to expect that.
Maybe it's that the word "offer" has sales connection to me, where things are exchanged for money in a trasactional way)
0. Open a profile of, and read about a few potential hosts, what they write about themselves and what other travellers wrote about staying with them, ..., get impression that you'll have good time together and maybe that you'd be reasonably safe with them

[comment]: <> (personally I don't consider safety as something to think actively about, I follow my intuition and unless something awakens my inner sense of danger-detection I just go for it. My default is to trust people I'd say. I am writing it because I wouldn't mention safety-verification here, but I don't know how others have. Just to consider removing it.)
0. Contact the potential hosts
0. Wait for their reply
0. When they reply, get a notification
0. They can host you!
0. Confirm
0. Enjoy the new real world connection, enjoy the time you spend together and the place, get some recommendations from a local where to go and what to do
0. After the stay, write a reference for them, and establish a friendship connection

Now, you have a home, you want to host travellers in your home:

0. Sign in
0. Write a little bit about yourself
0. Write about your home: approximate location, and say that you can host people
0. Wait for travellers to write to you
0. Somebody wrote! You received a notification. 
0. Read a message from them, read about them, maybe check experiences other hosts described having with them... and get excited about new people you're going to meet!
0. Invite them
0. They confirm
0. Enjoy the new real world connection, enjoy their stay
0. After their stay, write a reference for them, and establish a friendship connection

[comment]: <> (just as a side note: referrences are not a mandotory component, in WarmShowers they were hardly used, other platform may have different trust systems for example based on existing connections or profiles in other social networks. 
In Trustroots 'references' are called 'experiences' which I also personally prefer as 'references' seem to me a bit formal and like it was Linked-in or similar network. I belive the phrase 'references' comes from CouchSurfing but I never used it so idk)

---

In a more complex feature:

As a traveller:

0. Share your travel plans online
0. Maybe someone would like to invite you, they will write to you

As a host:

0. Look who's travelling into your area
0. Don't wait until they write to you. Invite them!

And that's the app we want to make!

[Next: App design](app-design.md)
