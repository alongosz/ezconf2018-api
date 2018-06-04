### Overview

I. Public API
- What is an API?
- What is and is not a part of API.
- Overview of eZ Platform PHP API services.
- Accessing Public API Services.
- Why to use API?
- Semantic Versioning and BC promise.
- Example BC-guaranteed usages.
- SPI and differences with API.
- Repository examples (PHP API).


### What is an API?

API - _Application Programming Interface_ - is a set of well defined and maintained methods for building application software.

### eZ Platform Public API:
- is a PHP API, grouped into Services with methods, allowing **Content Repository** manipulation,
- is provided by `ezpublish-kernel` package via the interfaces in `eZ\Publish\API` PHP namespace.

Note: it provides building blocks


### What belongs to eZ Platform API

- Repository Services defined by interfaces in the `eZ\Publish\API\Repository` namespace.
- Value Objects defined in the `eZ\Publish\API\Repository\Values` namespace.
- Other interfaces defined in the `eZ\Publish\API\Repository` namespace.


### What does NOT belong to<br> eZ Platform API

- Twig templates.
- Database schema (though there is still a separate BC promise for that).
- Anything located in `eZ\Publish\Core` namespace.

Note: mention that there are exceptions when it comes to Core.


### Available Repository Services:

```
- Content Service.
- Location Service.
- Content Service.
- Permission Resolver.
- Field Type Service.
- Language Service.
- Object State Service.
- Role Service.
- Search Service.
- Section Service.
- Trash Service.
- URL Alias Service.
- User Service.
```

Note: URLWildcardService is not mentioned as it is not used.


### Accessing Public API Services

Getting Content Service:
- in a Symfony Container-aware class:
```php
/** @var \Symfony\Component\DependencyInjection\ContainerInterface $container */
$repository = $container->get('ezpublish.api.repository');
/** @var \eZ\Publish\API\Repository\ContentService $contentService */
$contentService = $repository->getContentService();
```
- in a Controller extending `\eZ\Bundle\EzPublishCoreBundle\Controller` (not recommended):
```php
/** @var \eZ\Publish\API\Repository\ContentService $contentService */
$contentService = $this->getRepository()->getContentService();
```
Note: not recommended - to keep Controller class light, it shouldn't extend anything


Getting Content Service:
- via Repository injected into Symfony service:
```yaml
App\MyService:
  - '@ezpublish.api.repository'
```
- by injecting specific Repository service definition:
```yaml
App\MyService:
  - '@ezpublish.api.service.content'
```


### Why use an API?

- Any changes to application state (mostly database operations changing state of Content) are maintained and supported.
- It ensures all necessary layers (e.g. cache) are properly handled by the _Content Framework_.
- Has a _Backward Compatiblity_ promise on usage (for a _Consumer_).

Note: we'll get back in a moment to explaining what is an API Consumer.


### Semantic Versioning

eZ Platform packages, including<br /> `ezpublish-kernel`
follow the SemVer version numbering convention:

X.Y.Z (e.g. 2.1.1)

where:
- X is major version (release).
- Y is minor version.
- Z is patch version.

For more information see [https://semver.org/](https://semver.org/).


### BC promise (X.Y.Z)

Backward Compatibility promise is related mostly to changes between minor (Y) versions.

**Any minor release Y will not break existing code written for major version X.**

#### eZ Platform API

We ensure that any usage of API from the Consumer point of view will work across any minor (Y) release.


### Examples of BC and non-BC guaranteed usages:

Backward Compatibility is guaranteed for:
```php
class MyService
{
  /** @var \eZ\Publish\API\Repository\ContentService */
  private $contentService;

  public method doSomething()
  {
    // ...
    $this->contentService->publishVersion($versionInfo);
    // ...
  }
}
```


Backward Compatibility is **NOT** guaranteed for:
```php
use eZ\Publish\API\Repository\ContentService;

class MyService implements ContentService
{
  public method doSomething()
  {
    // ...
  }
}
```

**API that allows extending or implementing itself by 3rd party software is called SPI**


### Service Provider Interface

- Special kind of API maintained for Developers extending it.
- BC promise for SPI guarantees that classes extending or implementing it will not break across minor versions.
- eZ Platform provides SPI interfaces and abstract classes in the `eZ\Publish\SPI\` namespace.
- Exception: `eZ\Publish\SPI\Persistence` namespace which can change across minor versions due to API persistence layer needs.
