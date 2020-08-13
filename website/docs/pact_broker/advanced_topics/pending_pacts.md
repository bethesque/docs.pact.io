---
title: Pending pacts
---

The "pending pacts" feature allows changed contracts to be verified without failing the provider's build.

You can read about the feature here. [http://blog.pact.io/2020/02/24/how-we-have-fixed-the-biggest-problem-with-the-pact-workflow/](http://blog.pact.io/2020/02/24/how-we-have-fixed-the-biggest-problem-with-the-pact-workflow/)

Note that the pending feature is only applicable to the [provider changed workflow](/pact_nirvana/step_4#d-configure-pact-to-be-verified-when-provider-changes), where the list of pacts to be verified are fetched from the Pact Broker. It does not apply to the [contract changed](/pact_nirvana/step_4#e-configure-pact-to-be-verified-when-contract-changes) workflow where the pact URL is passed to the job via the webhook. This is because the pending status of the pact content is calculated based on the tags that the provider will use to publish the verification results, and the 'contract content changed' webhook cannot know this information.

## To start using the Pending pacts feature

* You need to either be using [pactflow.io](https://pactflow.io?utm_source=ossdocs&utm_campaign=pending_pacts), or have the latest version of the OSS Pact Broker running with the environment variable `PACT_BROKER_FEATURES=pacts_for_verification`
* You need to be on the latest version of the Pact client library for JVM, Javascript, Go, Ruby, .Net or Python (pact-python, not pactman)
* You need to find the verification documentation for your langage, and set the "enablePending" flag to true.
