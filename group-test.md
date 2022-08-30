# Test capabilities of Solid Groups

## The plan

1. Make a public group
    1. See if we can authorize the group members to access a resource
1. Make the group private - self-referencing (The group can be viewed by its members)
    1. See if we can see other members
    1. See if we can authorize the group members to access a resource

We want to make private groups such that the group members are visible only to each other, and can share their hosting offers only with each other.

And we're curious how different Solid Pod implementations handle this - NSS, CSS, ESS

## Setup

1. Make 2 accounts each on https://solidcommunity.net (NSS), https://solidweb.org (NSS), https://solidweb.me (CSS), https://login.inrupt.com (ESS)

1. Make 1 public and 1 private group on one of the accounts of each provider. The group will contain all created accounts' webIds.

1. Make documents on each account to share with each group, and see who can get access

### 1. Accounts

#### solidcommunity.net (NSS)
- https://grouptest1.solidcommunity.net/profile/card#me
- https://grouptest2.solidcommunity.net/profile/card#me

One needs to add the triple `:me solid:oidcIssuer <https://solidcommunity.net>`, if it wasn't added automatically, to assure compatibility with CSS

#### solidweb.org (NSS)
- https://grouptest1.solidweb.org/profile/card#me
- https://grouptest2.solidweb.org/profile/card#me

One needs to add the triple `:me solid:oidcIssuer <https://solidweb.org>`, if it wasn't added automatically, to assure compatibility with CSS

#### solidweb.me (CSS)
- https://solidweb.me/grouptest1/profile/card#me
- https://solidweb.me/grouptest2/profile/card#me

#### inrupt.com (ESS)
- https://id.inrupt.com/grouptest2
- https://id.inrupt.com/grouptest3

### 2. Groups

Each group looks as follows:

```turtle
@prefix : <#>.
@prefix vcard: <http://www.w3.org/2006/vcard/ns#>.
@prefix p1: <https://grouptest1.solidcommunity.net/profile/card#me>.
@prefix p2: <https://grouptest2.solidcommunity.net/profile/card#me>.
@prefix p3: <https://grouptest1.solidweb.org/profile/card#me>.
@prefix p4: <https://grouptest2.solidweb.org/profile/card#me>.
@prefix p5: <https://solidweb.me/grouptest1/profile/card#me>.
@prefix p6: <https://solidweb.me/grouptest2/profile/card#me>.
@prefix p7: <https://id.inrupt.com/grouptest2>.
@prefix p8: <https://id.inrupt.com/grouptest3>.

:group
    a vcard:Group;
    vcard:hasMember p1:, p2:, p3:, p4:, p5:, p6:, p7:, p8:.
```

Public groups have .acl file that looks as follows:

```turtle
@prefix : <#>.
@prefix acl: <http://www.w3.org/ns/auth/acl#>.
@prefix foaf: <http://xmlns.com/foaf/0.1/>.
@prefix c: </profile/card#>.

:ControlReadWrite
    a acl:Authorization;
    acl:accessTo <public-group.ttl>;
    acl:agent c:me;
    acl:mode acl:Control, acl:Read, acl:Write.
:Read
    a acl:Authorization;
    acl:accessTo <public-group.ttl>;
    acl:agentClass foaf:Agent;
    acl:mode acl:Read.
```

Private groups have .acl file that looks as follows:

```turtle
@prefix : <#>.
@prefix acl: <http://www.w3.org/ns/auth/acl#>.
@prefix c: </profile/card#>.
@prefix priv: <private-group.ttl#>.

:ControlReadWrite
    a acl:Authorization;
    acl:accessTo <private-group.ttl>;
    acl:agent c:me;
    acl:mode acl:Control, acl:Read, acl:Write.
:Read
    a acl:Authorization;
    acl:accessTo <private-group.ttl>;
    acl:agentGroup priv:group;
    acl:mode acl:Read.
```

#### NSS
- https://grouptest1.solidcommunity.net/group-test/public-group.ttl#group
- https://grouptest1.solidcommunity.net/group-test/private-group.ttl#group

#### CSS
- https://solidweb.me/grouptest1/group-test/public-group.ttl#group
- https://solidweb.me/grouptest1/group-test/private-group.ttl#group

#### ESS
- https://storage.inrupt.com/10b853a0-ce82-4500-9462-62a0e7b374d0/group-test/public-group.ttl#group (different access list)
- https://storage.inrupt.com/10b853a0-ce82-4500-9462-62a0e7b374d0/group-test/private-group.ttl#group (didn't manage to allow the group to see itself)

At this point we decide to give up on ESS (Inrupt provider), because we haven't found a way to give access to a group

## Results

### Can a person see a private group they're member of but don't own?

#### [person on solidcommunity.net](https://grouptest2.solidcommunity.net/profile/card#me) - [group on solidcommunity.net](https://grouptest1.solidcommunity.net/group-test/private-group.ttl#group)

Result: YES!

#### [person on solidweb.org](https://grouptest2.solidweb.org/profile/card#me) - [community on solidcommunity.net](https://grouptest1.solidcommunity.net/group-test/private-group.ttl#group)

Result: YES!

#### [person on solidweb.me](https://solidweb.me/grouptest1/profile/card#me) - [community on solidcommunity.net](https://grouptest1.solidcommunity.net/group-test/private-group.ttl#group)

Result: Request `GET https://grouptest1.solidcommunity.net/group-test/private-group.ttl` **fails** with status `500 Internal Server Error` and body `normalizedAlgorithm.importKey is not a function`

This is probably identical issue to our [first side test](https://github.com/mrkvon/developing-distributed-app/blob/main/group-test.md#person-on-solidcommunity-sharing-file-with-person-on-solidwebme) which found out that authenticating to NSS with CSS fails in simpler cases, too.

#### [person on solidcommunity.net](https://grouptest2.solidcommunity.net/profile/card#me) - [community on solidweb.me](https://solidweb.me/grouptest1/group-test/private-group.ttl#group)

Result: Request fails with `403 Forbidden`.

#### [person on solidweb.me](https://solidweb.me/grouptest2/profile/card#me) - [community on solidweb.me](https://solidweb.me/grouptest1/group-test/private-group.ttl#group)

Result: Request `GET https://solidweb.me/grouptest1/group-test/private-group.ttl` **fails** with status `403 Forbidden` and body `{"name":"ForbiddenHttpError","message":"","statusCode":403,"errorCode":"H403"}`

Issue: https://github.com/CommunitySolidServer/CommunitySolidServer/issues/1442

### Can person see a document shared with a _public_ group they're member of?

#### [person on solidcommunity.net](https://grouptest2.solidcommunity.net/profile/card#me) - [group on solidcommunity.net](https://grouptest1.solidcommunity.net/group-test/public-group.ttl#group) - [document on solidweb.org](https://grouptest1.solidweb.org/group-test/document-public-group-solidcommunity.ttl)

Result: YES!

#### [person on solidweb.org](https://grouptest1.solidweb.org/profile/card#me) - [group on solidcommunity.net](https://grouptest1.solidcommunity.net/group-test/public-group.ttl#group) - [document on solidweb.me](https://solidweb.me/grouptest1/group-test/document-public-group-solidcommunity.ttl)

Result: YES!

#### [person on solidcommunity.net](https://grouptest1.solidcommunity.net/profile/card#me) - [group on solidweb.me](https://solidweb.me/grouptest1/group-test/public-group.ttl#group) - [document on solidweb.me](https://solidweb.me/grouptest2/group-test/document-public-group-solidweb-me.ttl)

Result: YES!

#### [person on solidcommunity.net](https://grouptest2.solidcommunity.net/profile/card#me) - [group on solidweb.me](https://solidweb.me/grouptest1/group-test/public-group.ttl#group) - [document on solidcommunity.net](https://grouptest1.solidcommunity.net/group-test/document-public-group-solidweb-me.ttl)

Result: YES!

#### [person on solidweb.me](https://solidweb.me/grouptest1/profile/card#me) - [group on solidcommunity.net](https://grouptest1.solidcommunity.net/group-test/public-group.ttl#group) - [document on solidweb.me](https://solidweb.me/grouptest2/group-test/document-public-group-solidcommunity.ttl)

Result: YES!

#### person on solidweb.me - group on solidweb.me - document on solidcommunity.net

Don't even try... CSS person can't authenticate to NSS (See [side tests](https://github.com/mrkvon/developing-distributed-app/blob/main/group-test.md#person-on-solidcommunity-sharing-file-with-person-on-solidwebme))

#### [person on solidweb.me](https://solidweb.me/grouptest1/profile/card#me) - [group on solidweb.me](https://solidweb.me/grouptest1/group-test/public-group.ttl#group) - [document on solidweb.me](https://solidweb.me/grouptest2/group-test/document-public-group-solidweb-me.ttl)

Result: YES!

### Can person see a document shared with a _private_ group they're member of?

The only meaningful test, at this point, is using NSS only. We expect any other combination to fail.

#### [Person on solidcommunity.net](https://grouptest2.solidcommunity.net/profile/card#me) - [group on solidcommunity.net](https://grouptest1.solidcommunity.net/group-test/private-group.ttl#group) - [document on solidweb.org](https://grouptest1.solidweb.org/group-test/document-private-group-solidcommunity.ttl)

The first issue appeared when UI didn't allow us to give access to the private group, so we edited [the acl file](https://grouptest1.solidweb.org/group-test/document-private-group-solidcommunity.ttl.acl) directly with

```turtle
@prefix : <#>.
@prefix acl: <http://www.w3.org/ns/auth/acl#>.
@prefix c: </profile/card#>.
@prefix priv: <https://grouptest1.solidcommunity.net/group-test/private-group.ttl#>.

:ControlReadWrite
    a acl:Authorization;
    acl:accessTo <document-private-group-solidcommunity.ttl>;
    acl:agent c:me;
    acl:mode acl:Control, acl:Read, acl:Write.
:Read
    a acl:Authorization;
    acl:accessTo <document-private-group-solidcommunity.ttl>;
    acl:agentGroup priv:group;
    acl:mode acl:Read.
```

Result: **fails** with `403 Forbidden`

We even tried to access the document with the [person who owns the group](https://grouptest1.solidcommunity.net/profile/card#me), without success.

Issue: https://github.com/nodeSolidServer/node-solid-server/issues/1699

---

### A side test:

#### [Person on solidcommunity](https://grouptest1.solidcommunity.net/profile/card#me) sharing [file](https://grouptest1.solidcommunity.net/group-test/document-person-solidweb-me.ttl) with [person on solidweb.me](https://solidweb.me/grouptest1/profile/card#me)

Result: **fails** with `500 Internal Server Error` and body `normalizedAlgorithm.importKey is not a function`

Issue: https://github.com/nodeSolidServer/node-solid-server/issues/1698

#### [Person on solidweb.me](https://solidweb.me/grouptest1/profile/card#me) sharing [file](https://solidweb.me/grouptest1/group-test/document-person-solidcommunity.ttl) with [person on solidcommunity](https://grouptest1.solidcommunity.net/profile/card#me)

Result: YES!

Issue: https://github.com/CommunitySolidServer/CommunitySolidServer/issues/1441 (resolved by adding missing `solid:oidcIssuer` to profile documents on NSS)

## Conclusion

We tested capabilities of authorization with Solid groups with NSS, CSS and ESS Solid servers. We gave up on ESS soon, because we didn't figure out how group permissions worked on ESS.

We found out multiple things about groups and authentication:

- in general, authentication with CSS account to NSS is buggy (on server side of NSS). The other way works, if correct `solid:oidcIssuer` is present in the NSS account's profile document.
- giving document access to members of public groups generally works (with exception of the above incompatibility)
- members of private groups can see their group only if the group is on NSS. We couldn't test CSS reading NSS group, because of the above incompatibility
- giving document access to members of private groups fails everywhere

We also opened github issues based on these tests:

- https://github.com/nodeSolidServer/node-solid-server/issues/1698
- ~~https://github.com/CommunitySolidServer/CommunitySolidServer/issues/1441~~ (got resolved by adding `solid:oidcIssuer`)
- https://github.com/CommunitySolidServer/CommunitySolidServer/issues/1442
- https://github.com/nodeSolidServer/node-solid-server/issues/1699
