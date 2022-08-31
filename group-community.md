# Making a community

_Here we figure out how to make a group. We join the group. We find other people of the group. It becomes a hospex community._

We've put a pretty arbitrary data model together. We hope (without any foundation) that other developers, who publish and use data related to hospitality exchange, will somehow discover and use our data model, too. This is the elusive hope for interoperability.

---
## _Issue: Will anybody ever use our data model?_

_We certainly doubt so. But we push these worries aside and keep going. We vaguely imagine that developers could share their data models in a common repository, something like NPM, but for Linked Data Vocabularies... It would be easily searchable, and vocabularies could be discussed, and people would somehow, magically, come to an agreement about their data needs. There already exists the [Linked Open Vocabularies (LOV)](https://lov.linkeddata.es/dataset/lov/), but that doesn't seem to be enough._

_Well, now, as i'm writing this, i've [submitted](https://lov.linkeddata.es/dataset/lov/suggest) our [hospex vocabulary](http://w3id.org/hospex/ns) to LOV. Let's see if they accept and publish it there..._

---

## How do we find each other?

I can offer people a place to stay. But how do they ever find me? At the moment, our data live in different places, and we have no way to find each other. Internet is a vast place...

### 1. Crawl

Well, people can look at their friends, and friends of friends, and friends of friends of friends, and so on, and see if somebody offers a place to stay; and where it is.
This would only work if the network of friends is dense and well established. But if i'm new to the network, or if somebody is new, they won't find me that way.

We could use a network of hospex contacts (To Be Done) to perform the same search.

#### Advantages

- We host people in our friends network, so we can feel safer with them
- We don't need any additional search system

#### Disadvantages

- New people can't find others, and others can't find them
- Networks may be disjoint (i.e. two or more disconnected groups), and therefore not know about each other, essentially making separate communities
- Offers have to be public. Currently Solid doesn't have any mechanism to give permissions to friends of friends of friends of friends, and nobody else. (We could make a crawler that collects these "foafoafoafs" to a single group, and give permissions to that group.)
- We don't have any notion of a Community (e.g. cyclists, hitchhikers, ...) - like [different](https://welcometomygarden.org/) [hospex](https://www.bewelcome.org/) [communities](https://www.facebook.com/groups/hostasister/), or different [Trustroots Circles](https://www.trustroots.org/circles)

### 2. Make a place to discover each other

Create a place (a group) where people can register. Members can find each other in this group.

Improvement #2.1: Make the group members searchable by location, creating a geo index

Advantages:
- People who previously didn't know each other can find each other
- Solid has a way of sharing permissions with groups of people - our data don't have to be public - we only share our offers with members of our community or communities (well, ehm, we'll see about that in a moment...)

Disadvantages:
- Centralized solution, a single point of failure, a single point of control
- If we don't make _Improvement #2.1_, we have to fetch all members' offers in order to find hosts in the area we travel to


### 3. Make a distributed way to discover each other (Distributed Hash Tables)

I don't know what i'm talking about... just using some trendy words here. Only in most vague and foggy way can i imagine how this would function

Somehow, everybody would host a part of the group/network/index on their own Solid Pod, and somehow, magically, people could find each other that way

#### Advantages
- distributed system, no central point of failure

#### Disadvantages
- complicated, hard to understand
- no clear way to manage permissions (do data have to be public again?)

---

These are all valid options. For starters we went for the middle option - making a centralized group to discover each other


## How to make a group

We hope to create a group such that

- Only group members can fetch the list of other members
- Some groups will be accepting everybody, others will be private - invitation only, or you apply for membership and somebody has to accept you
- I can decide to only share my offers with the members of the group, not with the whole world
- It's possible to leave the group

We've seen in some Solid Server user interfaces, that we can give permissions to a group, so we are hopeful

![Add Group](assets/nss_add_group.png)

### Figuring out how Solid groups work

We tried to find examples of existing Solid groups, without much success. So we turned to [Solid specification](https://solidproject.org/TR/protocol), and here we've had more luck. The section [Authorization](https://solidproject.org/TR/protocol#authorization) directed us to [WAC specification](https://solidproject.org/TR/wac). There, we found notion of _[agent group](https://solidproject.org/TR/wac#agent-group)_, and finally, in a [section called Access Subjects](https://solidproject.org/TR/wac#access-subjects), we read

> The `acl:agentGroup` predicate denotes a group of agents being given the access permission. The object of an `acl:agentGroup` statement is an instance of `vcard:Group`, where the members of the group are specified with the `vcard:hasMember` predicate.

Great! So the group should look like

```ttl
@prefix : <#>.
@prefix vcard: <http://www.w3.org/2006/vcard/ns#>.
@prefix p1: <https://person1.inrupt.net/profile/card#>.
@prefix p2: <https://person2.solidcommunity.net/profile/card#>.
@prefix p3: <https://person3.solidweb.org/profile/card#>.
@prefix p4: <https://person4.org/profile/card#>.

:group
    a vcard:Group;
    vcard:hasMember p1:me, p2:me, p3:me, p4:me.
```

(remember how to read this? [Triples, triples, triples...](my-profile.md#triples-triples-triples))


### Testing the group out

Now, let's see if this actually works and if we can use it.

We want to:

1. Make a public group
    1. See if we can authorize the group members to access a resource
1. Make the group private - self-referencing (The group can be viewed by members of itself)
    1. See if we can see other members
    1. See if we can authorize the group members to access a resource

Because ideally, the group members are visible only to each other, and can share their hosting offers only with each other.

And we're curious how different Solid Pod implementations handle this - NSS, CSS, ESS

We put a [detailed report into a separate document](group-test.md).

#### What did we learn?

Some results of this testing are hopeful, some a bit discouraging.

We've found that groups, as a concept, work. We've also encountered some [bugs](https://github.com/nodeSolidServer/node-solid-server/issues/1698), which can be fixed.

But unfortunately, private groups [probably don't work according to spec](https://github.com/CommunitySolidServer/CommunitySolidServer/issues/1442#issuecomment-1229842295). I've [pursued this](https://forum.solidproject.org/t/can-i-use-a-non-public-group-to-define-access-to-my-resources/4841) [question further](https://gitter.im/solid/specification?at=630cef82443b7927a7eaaa83), but it's disappointing.

Basically, at this point, the options are two:

1. Everybody in the internet can see the list of hospitality exchange members
2. Group is private, but the people's hospex data are public. ([this doesn't work if the group is hosted on CSS](https://github.com/CommunitySolidServer/CommunitySolidServer/issues/1442))

We chose the second option to start. But it's a difficult choice, two suboptimal options...

### Finally, making it

We made the following data design for a group:

![hospex data model](https://raw.githubusercontent.com/OpenHospitalityNetwork/ohn-solid/main/docs/assets/data-model.jpg)

And we added 2 new words to our hospex vocabulary: [memberOf](http://w3id.org/hospex/ns#memberOf), and [Community](http://w3id.org/hospex/ns#Community).

Let's make a public document with community name and description, and a private document with community members.

The public document:

```ttl
@prefix : <#>.
@prefix sioc: <http://rdfs.org/sioc/ns#>.
@prefix mem: <members#>.

:us
    a sioc:Community;
    sioc:about
    "Sleepy Bike is a hospitality exchange community for cyclists"@en;
    sioc:has_usergroup mem:group;
    sioc:name "Ospal\u00e9 kolo"@cs, "Sleepy Bike"@en.
```

The private document:

```ttl

```






_to write: test if group works, test if it can be private and members can still fetch other members, test if data can be shared with the private group only (+ test different pod implementations), make a private group, allow anybody to append, describe limitations we start with (public offer, append only, no leaving, no search by location), implementing finding groups, searching groups, showing people on a map_








