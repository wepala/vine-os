# Pod Planing Stories

## Linked Data Platform (LDP)
These stories describe the functionality of a Linked Data Platform (LDP) pod, which is a personal data store that allows users to manage and share their data in a decentralized manner.
The stories focus on the LDP features, such as resource management, folder organization, and data formats. Auth(z)(n) will 
be handled by a separate Identity and Access Management (IAM) service.

### User Stories
* I should be able to see a list of resources available in the pod so that I can choose what to use.
* I should be able create a new resource by passing in JSON-LD data so that I can add new information to my pod.
* I should be able to create a folder within my pod so that I can organize my resources.
* I should be able to delete resources and folders
* I should be able to see psuedo folders based on the types of resources
* I should be able to see a list of resources in a folder
* I should be able to download a resource in different formats (e.g. JSON-LD, Turtle, RDF/XML)
* I should be able to add external resources (e.g. from other pods or the web) to my pod so that I can link to them.
* I should be able to update resources in my pod by passing in new JSON-LD data
* I should be able to see the metadata of a resource (e.g. creation date, last modified date, size)

### Admin Stories
* I should be able to configure the pod to allow or disallow public access to resources
* I should be able to set storage quotas for users
* I should be able to configure what content types are allowed in the pod
* I should be able to configure what storage backends are used (e.g. local filesystem, S3, etc.)
* I should be able to configure transactional email services