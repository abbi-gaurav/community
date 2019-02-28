# Overview

As an operator, I want to configure events coming from an external application or a source to be stored in one of the many Messaging middlewares available in a running Kyma cluster.

As a developer, I want to configure an event coming from an external application or a source to be used for triggering my awesome serverless extension. I do not concern myself with underlying details of event storage.

To have a better UX, while still being flexible, it would be better to have multiple levels of configuration with the most fine grained one overriding others.

## Default

If there is no configuration specified, a system wide [default](link to default proposal) will be used.

## Messaging middleware per Kyma application

When a new application is provisioned inside Kyma, it should be possible to state which Messaging middleware(ClusterChannelProvisioner) will be used for all the events originating from this application.

**Why**

* The semantics sits well with this being an operator concern.
* Some applications will have affinity to a definitive Messaging middleware. There could be use cases where a customer is already using a cloud Messaging middleware and she would like to use the same for all his application events.
* The decision is governed by event production.

**Open Questions**

* How updates will be handled?
  * A change at this level could trigger big chain reaction which includes changing all channels, subscriptions at the lower level.
* How to handle deprovisioning of applications?
  * On deprovisioning, all the channels, Kyma subscriptions, Knative subscriptions will be deleted.

**Proposed Approach**

* Start with the configuration being a one time activity for now. This is in-sync with Knative as  they follow similiar approach for defaults.
* The possibility to update or change will be envisioned later on.

**Requirements**

* An API to get all the ClusterChannelProvisioners (Messaging middlewares).
* A mechanism to store such a mapping.
* Implementation update to take into account such a mapping while creating Knative channels.

### Troubleshooting support

Where things can go wrong?

* An operator configured a wrong Messaging middleware?
  * In such a case, there needs to be a migration path either automatic or manual.
  * e.g. there can be an API to deprovision which will delete all the related resources (Knative channels, Kyma Subscriptions, Knative subscriptions and others if any).

* A User Interface where all the information including mapping should be displayed.

## Messaging middleware close to event definition

-TBD-
