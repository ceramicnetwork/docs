# Authorizations

Authorization is the act of delegating access to a stream to an account that is different than its owner. As a best practice, when granting authorizations to another account you want to follow the rule of least privilege and only authorize that delegate's temporary key to write the minimally needed data to Ceramic. 

## Scopes

---

CACAO and Ceramic support a basic way to describe the resources and actions an authorization includes. The resource parameter is an array of strings. In Ceramic those strings are StreamIDs or model StreamIDs. The implied action is write access, as read access is not authorized in any way at the protocol level. Read access would require an encryption protocol, as streams are public, and is out of scope for now. 

!!! note
    In the future, we expect the ability to specify more granular authorizations based on actions (write, delete, create, update etc) and resources.

### Streams

For example, to authorize an account to write to only two specific streams, you would specify the streamIds as resources in the CACAO as follows:

```jsx
[ "ceramic://kjzl6cwe1jw14bby1eybtqjr1w5l8xysitwmd34i8huccr7lk8g6xrt2l1c1ngn", "ceramic://kjzl6cwe1jw1476bbp2a0lg8gcmk9zj1xjanpg6dooc3golyb2fnmwmg0p6ane3"]
```

### Models

The mostly commonly used pattern is to specify authorizations by model streamIds. `model` is a property that can be defined in a streams init event. When specified and used with CACAO it allows a DID and key the ability to write to all streams with this specific model value for that user. 

!!! note
    Ceramic will likely support other keys and values in streams beyond `model` for authorizations in the future.


Models at the moment are primarily used as higher level concept built on top of Ceramic. A set of models will typically describe the entire write data-model of an application, making it a logical way for a user to authorize an application to write to all streams that is needed for that application. 

For example, a simple social application with a user profile and posts would have two corresponding models, a profile model and a post model. The CACAO would have the resources specified by an array of both model streamIds, shown below. This would allow a DID with this CACAO to create and write to any stream with these models. Allowing it to create as many posts as necessary. 

Resources defined by model streamID are formatted as `ceramic://*?model=<StreamId>` and would be defined as follows for the prior example. 

```jsx
[ "ceramic://*?model=kjzl6hvfrbw6c7keo17n66rxyo21nqqaa9lh491jz16od43nokz7ksfcvzi6bwc", "ceramic://*?model=kjzl6hvfrbw6c99mdfpjx1z3fue7sesgua6gsl1vu97229lq56344zu9bawnf96"]
```

### Wildcard

Lastly a wildcard for all resources is supported. For security reasons, wildcard will be deprecated in the future and is only included here for completeness. 

```jsx
[ "ceramic://*" ]
```