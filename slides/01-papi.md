### Overview

I. Public API
- What is an API?
- Semantic Versioning and BC promise.
- Injecting Repository services into Symfony application.
- Overview of eZ Platform PHP API services.
- Repository examples (PHP API).
- Differences between API and SPI.


### What is an API?

API - _Application Programming Interface_ - is a method or set of well defined and maintained methods allowing to perform certain operations.

### eZ Platform Public API:
- is a PHP API, grouped into Services with methods, allowing **Content Repository** manipulation,
- is provided by `ezpublish-kernel` package via the interfaces in `eZ\Publish\API` PHP namespace.


### Why use an API?

- Any changes to application state (mostly database operations changing state of Content) are maintained and supported.
- It ensures all necessary layers (e.g. cache) are properly handled by the _Content Framework_.
- Has a _Backward Compatiblity_ promise on usage (for a _Consumer_).


### What belongs to eZ Platform API

- Repository Services defined by interfaces in `eZ\Publish\API\Repository` namespace.
- Value Objects defined in `eZ\Publish\API\Repository\Values` namespace.


### What does NOT belong to eZ Platform API

- Templates.
- Database schema (though there is still a separate BC promise for that).
- Anything located in `eZ\Publish\Core` namespace.
