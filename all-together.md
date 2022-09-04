# Putting it all together

*A story of a user*

If you want to join the [sleepy.bike](https://sleepy.bike), you need a Solid Pod - it will provide your identity, and the app will store your data there.

If you don't have one already, you can choose from a few readily available options:

[Node Solid Server](https://github.com/nodeSolidServer/node-solid-server):
- https://solidcommunity.net
- https://solidweb.org
- https://inrupt.net

[Community Solid Server]():
- https://solidweb.me

[Inrupt Enterprise Server]():
- https://start.inrupt.com/profile

## Sign in for the first time

With this profile, let's say it's https://mynewprofile.solidcommunity.net/profile/card#me, you sign in to https://sleepy.bike for the first time.

The app will check whether you have a file `/public/hospex.ttl` in your pod. If you don't have that file, it will create it and start introducing you to the app.

## Fill your profile

This is not implemented, yet. You can fill your profile directly in your Solid Pod - name, description. They'll show up in the app.

## Join a community

There is a [hardcoded](https://github.com/OpenHospitalityNetwork/ohn-solid/blob/05502fc3b2da6840bc41265426ba1f3f021e13d0/src/App.tsx#L30) default community [Sleepy Bike](https://mrkvon.inrupt.net/hospex/sleepy-bike/community#us) in the app, and it will show up in the community list.

You can join it by clicking `JOIN` button.

At that point a request to append you as a member is sent to the community's group:

`PATCH https://mrkvon.inrupt.net/hospex/sleepy-bike/members`

with body

```sparql
INSERT DATA {
  <https://mrkvon.inrupt.net/hospex/sleepy-bike/members#group> <http://www.w3.org/2006/vcard/ns#hasMember> <https://mynewprofile.solidcommunity.net/profile/card#me> .
}
```

If everything works well, your webId will get appended to the list of members.

Also, your own `/public/hospex.ttl` will get updated with triple `<i> <am a member of> <Sleepy Bike community>.`, or

```ttl
@prefix hospex: <http://w3id.org/hospex/ns#>.

</profile/card#me> hospex:memberOf <https://mrkvon.inrupt.net/hospex/sleepy-bike/community#us>.
```

From now on, you can read the list of community members.

And that's what happens next. The app downloads all members' hospex documents. If anybody is a member of any other community, the community will show up in the list as well. But it can take tens of seconds to get to see all communities this way.

## Create an offer

You joined a community. Now you want to host others. So you create a hosting offer.

You fill the form (description and location), and the following info gets added to your personal hospex document:

```
<I> <offer> <Offer>.

<Location> <is a> <Geographical point>.
<Location> <has latitude> 50.0.
<Location> <has longitude> 15.0.

<Offer> <is> <hospitality accomodation>.
<Offer> <has description> "Some info about my home"@in-english.
<Offer> <has location> <Location>.
<Offer> <is offered by> <I>.
```

in picture (TODO):

![TODO]()

```ttl
@prefix : <#>.
@prefix geo: <http://www.w3.org/2003/01/geo/wgs84_pos#>.
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>.
@prefix p: </profile/card#>.
@prefix hospex: <http://w3id.org/hospex/ns#>.

p:me hospex:offers :offer5678.

:location1234
    a geo:Point;
    geo:lat 50.0;
    geo:long 15.0.
:offer5678
    a hospex:Accommodation;
    rdfs:comment "Some info about my home"@en;
    geo:location :location1234;
    hospex:offeredBy c:me.
```

## Look at the map

To show the offers on the map, the app simply fetches all the people from your communities, then fetches their personal hospex documents (for now it's just `/public/hospex.ttl` - not very interoperable), and displays any offers it finds.

You can see that this is not scalable. With community of a million people, you would have to download all million profiles!

We'll need to implement an index to search by location.

## Show a person's profile

Just show the picture and description from personal profile document of the person...

---

And that's all folks! At least for now. Let's see what's awaiting us...

[Next: Next steps](next-steps.md)
