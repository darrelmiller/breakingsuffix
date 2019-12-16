# Breaking Suffix Versioning Strategy

## The problem

For as long as this industry has been trying to make computers talk to each other over networks we have been searching for an ideal way to manage change. Developers have experimented with many different approaches to use versioning to manage change.  The industry has largely come to the consensus that in a perfect world there would be no need to use versioning to manage change as evolving APIs is the most consumer friendly approach. This is not a perfect world. While some developers have found ways to make evolving APIs work for them and live with the additional complexity, the vast majority rely on some form of versioning to provide a periodic opportunity to reset.

## The last holdout

Since the transition to JSON over XML based payloads, the industry has accepted that, in most API scenarios, new identifiers can be added without breaking consumers. This makes additive changes simple to manage.  

More recently many API approaches have developed a way to annotate identifers with metadata that can communicate with consumers that the identifier will be deprecated at some point in the future.  This is a consumer friendly way of removing functionality that provides time for client applications to adapt to change.

Unfortunately, there is no standardized or even common approach to deal with existing identifiers that need to change their type, structure or behavior in a way that would break existing clients.  Convicted proponents of evolving APIs often choose the "thesaurus approach" of finding a new identifier with a similar name. This avoids breaking clients, but over time can introduce technical debt, complexity and potential confusion to an API.  Most API providers will use a version identifier at the beginning of the API path to signal a major version change to the API.  The instances where breaking changes are needed are accumulated over time until there are a sufficient number of significant changes to justify introducing the breaking change and starting the cycle of convincing API consumers to do the work necessary to update to the new major API version.

## The solution

Splitting a breaking change into two steps reveals a solution that has a number of benefits. The **first step** is to make the change non-breaking by adding a new identifier that is constructed using the original identifier plus a suffix that has a standard format. This is called the *breaking suffix*.  e.g. Add a property `skills_v2` to replace the existing property `skills`.

The **second step** is to remove the *breaking suffix* from new identifier. API providers that make major version changes can use this opportunity to remove the suffix.  API providers that only evolve APIs can use an additional deprecation cycle to remove the suffix and return to the original property name.

## The benefits

- Breaking changes can be introduced immediately. It is not necessary to wait for the next major version update, improving time to market.
- API level major version updates have less impact on consumers because next version features can be adopted incrementally and then transitioning to a new major version across the entire API can be a purely mechanical process.
- The standardized suffix can be detected automatically
- It is clear what identifier has been introduced to replace the original identifier.
- No tooling is required to implement this schema.
- Tooling could be used to help.
- APIs built by multiple teams no longer need to co-ordinate the release of their breaking changes.
- Makes it easier for multiple teams to share the same major version identifier and create a consolidated API for the company.  This is especially important for teams that have APIs that have dependencies on other teams in the company.

## Something for everyone

This solution, while far from a perfect world, does address the major change management problems of all participants, regardless of their approach to building APIs.  API consumers that are early adopters can start using new features sooner.  More conservative API consumers can defer adopting new features until they need to move off a major version that is going to be deprecated. They still get the benefit of adopting new features incrementally on a timeline that is more compatible with their own release schedule.  API consumers can choose to ignore all breaking suffix identifiers and do a big bang update when a major version update happens.

API providers get the freedom to introduce any kind of change at any time and consumers can adopt those changes at any time. This approach should make it simpler to convince consumers to update their applications to new versions. It becomes much easier for both providers and consumers to track what breaking changes have been introduced over time and when those changes are introduced into a major version.

Whether an API provider believes in evolving APIs, or using API level major version changes, both can benefit from the standard suffix and the tooling that could be built around it.

## Rules

- Any identifier defined by a networked based API can use the *breaking suffix* versioning strategy.  This includes identifiers such as path segments, query parameters, JSON parameters, XML elements.
- Add new identifiers to the API as required.
- Mark identifiers to be removed from the API as deprecated using an available metadata mechanism
- Breaking changes can be introduced to existing identifiers by adding a new identifier using the original identifier plus a breaking suffix ( "_vn" where n is the next major version number)
- The original identifier SHOULD be marked as deprecated.
- New major versions of an API MAY be introduced in order to provide the opportunity to remove deprecated identifiers and remove breaking suffixes.  

## Examples

- Need to change data type of JSON property `skills` from 
  - 