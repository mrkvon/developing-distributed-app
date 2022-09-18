# A technical reference: What's going on behind the scenes

## Requests sent to Solid Pod

In every example, there is first url to implementation, and then the actual request

### create hospex document

https://github.com/OpenHospitalityNetwork/ohn-solid/blob/4db3bb320e2ad750042421724e2689c45c7278a3/src/features/user/userAPI.ts#L103-L114

`PUT https://myusername.solidcommunity.net/public/hospex.ttl`

`(empty body)`

Response: `201`



### join group

https://github.com/OpenHospitalityNetwork/ohn-solid/blob/4db3bb320e2ad750042421724e2689c45c7278a3/src/features/community/communityAPI.ts#L30-L63

#### add self to group

`PATCH https://myusername.solidcommunity.net/public/hospex.ttl`

```sparql
INSERT DATA {
    <https://mrkvon.inrupt.net/hospex/sleepy-bike/members#group> <http://www.w3.org/2006/vcard/ns#hasMember> <https://myusername.solidcommunity.net/profile/card#me> .
}
```

Response: `200`

#### add group to self

`PATCH https://myusername.solidcommunity.net/public/hospex.ttl`

```sparql
INSERT DATA {<https://myusername.solidcommunity.net/profile/card#me> <http://w3id.org/hospex/ns#memberOf> <https://mrkvon.inrupt.net/hospex/sleepy-bike/community#us>.};
```

Response: `200`

#### create offer

https://github.com/OpenHospitalityNetwork/ohn-solid/blob/4db3bb320e2ad750042421724e2689c45c7278a3/src/features/offer/offerAPI.ts#L73-L105

`PATCH https://myusername.solidcommunity.net/public/hospex.ttl`

```sparql
INSERT DATA {<https://myusername.solidcommunity.net/public/hospex.ttl#offer1663522375800> a <http://w3id.org/hospex/ns#Accommodation>;
    <http://w3id.org/hospex/ns#offeredBy> <https://myusername.solidcommunity.net/profile/card#me>;
    <http://www.w3.org/2003/01/geo/wgs84_pos#location> <https://myusername.solidcommunity.net/public/hospex.ttl#location1663522407917>;
    <http://www.w3.org/2000/01/rdf-schema#comment> "This is just a test"@en.
<https://myusername.solidcommunity.net/public/hospex.ttl#location1663522407917> a <http://www.w3.org/2003/01/geo/wgs84_pos#Point>;
    <http://www.w3.org/2003/01/geo/wgs84_pos#lat> -67.60922060496382;
    <http://www.w3.org/2003/01/geo/wgs84_pos#long> -64.6875.
<https://myusername.solidcommunity.net/profile/card#me> <http://w3id.org/hospex/ns#offers> <https://myusername.solidcommunity.net/public/hospex.ttl#offer1663522375800>.};
```

Response: `200`

### update offer

https://github.com/OpenHospitalityNetwork/ohn-solid/blob/4db3bb320e2ad750042421724e2689c45c7278a3/src/features/offer/offerAPI.ts#L107-L132

`PATCH https://myusername.solidcommunity.net/public/hospex.ttl`

```
DELETE DATA {<https://myusername.solidcommunity.net/public/hospex.ttl#offer1663522375800> <http://www.w3.org/2000/01/rdf-schema#comment> "This is just a test"@en.
<https://myusername.solidcommunity.net/public/hospex.ttl#location1663522407917> <http://www.w3.org/2003/01/geo/wgs84_pos#lat> -67.60922060496382;
    <http://www.w3.org/2003/01/geo/wgs84_pos#long> -64.6875.}; INSERT DATA {<https://myusername.solidcommunity.net/public/hospex.ttl#offer1663522375800> <http://www.w3.org/2000/01/rdf-schema#comment> "This is just an edited test"@en.
<https://myusername.solidcommunity.net/public/hospex.ttl#location1663522407917> <http://www.w3.org/2003/01/geo/wgs84_pos#lat> -67.61366667379862;
    <http://www.w3.org/2003/01/geo/wgs84_pos#long> -64.73934173583984.};
```

Response: `200`

### remove offer

https://github.com/OpenHospitalityNetwork/ohn-solid/blob/4db3bb320e2ad750042421724e2689c45c7278a3/src/features/offer/offerAPI.ts#L134-L148

`PATCH https://myusername.solidcommunity.net/public/hospex.ttl`

```
DELETE DATA {<https://myusername.solidcommunity.net/public/hospex.ttl#location1663522407917> a <http://www.w3.org/2003/01/geo/wgs84_pos#Point>;
    <http://www.w3.org/2003/01/geo/wgs84_pos#lat> -67.61366667379862;
    <http://www.w3.org/2003/01/geo/wgs84_pos#long> -64.73934173583984.
<https://myusername.solidcommunity.net/public/hospex.ttl#offer1663522375800> a <http://w3id.org/hospex/ns#Accommodation>;
    <http://w3id.org/hospex/ns#offeredBy> <https://myusername.solidcommunity.net/profile/card#me>;
    <http://www.w3.org/2000/01/rdf-schema#comment> "This is just an edited test"@en;
    <http://www.w3.org/2003/01/geo/wgs84_pos#location> <https://myusername.solidcommunity.net/public/hospex.ttl#location1663522407917>.};
```

Response: `200`
