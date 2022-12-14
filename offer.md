# Offer

_We figure out how to make offer * and create it manually on pod * We fetch and show the offer_

## The Shape of the Offer

This is where it gets creative. We have a data structure to figure out.

First, let's think what we want to say: For a start, you want to say, approximately, where your home is. You want to write its short description, and you want to say whether you can host, currently.

So perhaps it could be:

0. I offer a place to stay for travellers.
0. This place is at location 50.1N, 14.9E.
0. You'll find there "Blablabla ... and more text".
0. I may be able to host at this place at the moment.

A bit simplified, it could go like this: (remember, it's short 3-word (Subject, Predicate, Object) sentences:

0. Me offers Offer.
1. Offer hasLocation Location.
    - Location hasLatitude: 50.1.
    - Location hasLongitude: 14.9.
2. Offer hasDescription "Blablabla ... and more text" (English).
3. Offer hasAvailability MaybeCanHost.

We just made this up. It could work differently. We could also add more structured stuff (which we will, for sure). For example, what do you offer? Room? Garden? ...

At this point you go hunting on the internet. Try to find out whether somebody talked about hospex in terms of Linked Data and RDF before. Or at least did they talk about descriptions, locations, or something?

Fortunately, there is a [convenient website](https://lov.linkeddata.es), which collects various RDF Vocabularies and Ontologies (sorry, more scary words. But these are just translations from normal language to URLs). So you look up terms like [Accommodation](https://lov.linkeddata.es/dataset/lov/terms?q=accommodation), [Offer](https://lov.linkeddata.es/dataset/lov/terms?q=offer), [Hospitality](https://lov.linkeddata.es/dataset/lov/terms?q=hospitality), Hospex, [Location](https://lov.linkeddata.es), [Description](https://lov.linkeddata.es/dataset/lov/terms?q=description), [About](https://lov.linkeddata.es/dataset/lov/terms?q=about), ...

Not much luck there with Hospitality-related topics[^hospex-update]. But we find a vocabulary [WGS84](http://www.w3.org/2003/01/geo/wgs84_pos), which describes earth coordinates. And we find a few [possible](http://purl.org/dc/terms/description) [terms](http://www.w3.org/2000/01/rdf-schema#comment) for the predicate `has description`. We'll need to make up the rest and create our own new vocabulary which we'll call _hospex_.

[^hospex-update]:
    On the second look, we've also found terms that we should include in our hospex vocabulary, if possible:
    - [Accommodation](https://schema.org/Accommodation)
    - somebody [offers](http://purl.org/goodrelations/v1#offers) something
    - [Offer](https://schema.org/Offer)

Here comes a simple rule of thumb: If a _vocabulary_ or a _word_ exists already, try reusing it. Ideally, we want one common language, so it's better to call one thing one (common) name.

```
Me hospex:offers Offer.
Offer hospex:offeredBy Me.
Offer rdf:type(?) hospex:Accommodation.
Offer wgs84:location Location.
Location rdf:type wgs84:Point.
Location wgs84:lat 50.1.
Location wgs84:lon 14.9.
Offer rdfs:comment "Blablabla ... and more text"@en.
Offer hospex:available hospex:Maybe. (Figure out!)
```

Me, Offer, and Location will be URLs, or URIs that represent ... :wink: ... me, the offer and its location...

What about the new hospex vocabulary? We leave it as a placeholder for now. Eventually, we'll write and [publish it](http://w3id.org/hospex/ns) to [w3id.org](https://w3id.org/) via [github](https://github.com/perma-id/w3id.org/pull/2397).


## Save the offer

First, we publish our example offer to our Solid pod. We don't bother making interface to save it from the app, yet. We create a new file called `hospex.ttl` in folder `public` (we'll deal with permissions later). And we copy the following data into that file.

```turtle
@prefix : <#>.
@prefix wgs84: <http://www.w3.org/2003/01/geo/wgs84_pos#>.
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>.
@prefix hospex: <http://w3id.org/hospex/ns#>.

</profile/card#me> hospex:offers :offer123456 .

<#location123456>
a wgs84:Point; wgs84:lat 50.1; wgs84:long 14.9 .
<#offer123456>
    a hospex:Accommodation;
    hospex:offeredBy c:me;
    rdfs:comment
        """Blablabla ... and more text"""@en;
    wgs84:location :location123456 .
```


## Show the offer

### Discovery unresolved

The challenge at the start: How does the app find where my offer data are stored?

And the answer is: Badly. Crappily. Without teeth. Solid Pod is a database, but you can't query it. It doesn't have a [LDF](https://linkeddatafragments.org/) endpoint, and it doesn't have a [SPARQL](https://www.w3.org/TR/sparql11-query/) endpoint. It's just folders with files with RDF, and you're supposed to find the data you're interested in somehow. The only thing that Solid offers is type index, where it should vaguely say in which file you can find things of type hospex:Accommodation. But this type index must go hopelessly out of sync.

I don't like this. [I think Solid Pods should have LDF endpoints](https://mrkvon.org/blog/solid-i-want/). It would fix the discoverability issue, which is one of the reasons why interoperability between apps is difficult. (another reason is using different words for the same thing)

The current situation shows that Specification writing is largely disconnected from using the Specification. [This already bothered Aaron Swartz](https://en.wikisource.org/wiki/A_Programmable_Web/Chapter_1#pagenumber_3).

So... anyways... let's end the rant here, leave it unresolved, and move on.

### Cover our eyes and skip discoverability :see_no_evil: :hear_no_evil: :speak_no_evil:

In our app, for the time being, we'll assume that the hospex data are stored in `/public/hospex.ttl`.

This happens to be a challenge on its own.

If one webId is https://myusername.solidcommunity.net/profile/card#me, other is https://solidweb.org/myusername/profile/card#me and another is https://webid.myusername.net, how do we construct the `/public/hospex.ttl` url? We take yet another shortcut (look into the code and describe).

### Fetch and display

And we fetch the data there, like we did with the profile. And we show them in UX


### Create, Update and Remove Offer Programmatically

We haven't looked how to edit the offer. Well, we just use @inrupt/solid-client again.

It's a very important piece, but a bit boring to describe it here, so if you're interested how it's done, read in our [technical reference](./reference.md).

There is always a link to the implementation, and then the request that gets sent (you don't have to do that manually!):

- [create offer](./reference.md#create-offer)
- [update offer](./reference.md#update-offer)
- [remove offer](./reference.md#remove-offer)

---

Great. But how can others find my offer?


:arrow_right: **[Next: A group. A community](group-community.md)**
