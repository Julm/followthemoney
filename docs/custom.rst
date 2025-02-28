Extending the schema
=====================

`followthemoney` (FtM) lives a double life, as a data modelling and validation tool, and
as a specific data model. While the default model is tailored to investigative
journalism, the data modelling tool is reusable beyond that domain. Users from adjacent
fields, such as human rights advocacy, cyber forensics or social media analysis might
wish to extend the data model.


Modelling principles
----------------------

The goal of `FtM` is to create a data model for international business, banking,
corporate law, government procurement, and social relations. It's important to be conscious
of how preposterous that premise is. In consequence, our goal cannot be to fully model these
domains, but to `model a conversation between two slightly drunk investigative reporters`
about these things.

Accordingly, the following heuristics should be applied to schema extensions:

* Does the schema change solve a practical precision issue in a dataset or in a client
  application, or will it merely serve to better reflect a legal theory?
* Can the framing of entities and their relations be expressed in such a way that they
  are applicable to more than one jurisdiction? (Jurisdiction-specific properties are OK)
* Avoid modelling out schema without different properties: we don't need `Doctor`,
  `Lawyer`, `TruckDriver`, `Farmer` as schemata, when this can just be a `profession`
  property.


Schema syntax
---------------

For a reference of how schema definitions are designed, use the following resources:

* Check out the existing YAML files in ``followthemoney/schema`` in the source
  repository.
* The documentation for :class:`followthemoney.schema.Schema` explains the options 
  available on for defining schema metadata.
* The documentation for :class:`followthemoney.property.Property` gives more detail
  regarding property descriptors.


Instance-specific overrides
-----------------------------

Operators can modify the active FtM model by setting the environment variable
``$FTM_MODEL_PATH`` to a directory that contains an alternate set of YAML schema
description files. The simplest way to do this would be to download the existing YAML
schema set from FtM source (`followthemoney/schema`), and make edits in a new directory. 

This raises an difficult question: how mutable is the FtM model in reality? FtM
uses specific schemata in its tests, but that wouldn't be a fatal hindrance.
However, the `aleph` and `ingestors` applications have built-in assumptions about a
larger subsection of the schema. For example, `ingestors` require all schemata descendant
from :ref:`schema-Document` and :ref:`schema-Mention`. Aleph assumes that the
:ref:`schema-LegalEntity`, :ref:`schema-Person` and :ref:`schema-Document` types exist.

In practice, we would recommend these policies when modifying the FtM model in an Aleph
deployment:

* Add schemata and properties, don't remove or rename the existing ones. 
* Don't add too many schemata. Each schema will add a new index in Aleph's ElasticSearch,
  and each index needs to be queried individually. Add 10 or 20 schemata, not 200.
* Contribute upstream: once you have successfully modelled some data into an extension
  of FtM, please feel free to submit a pull request to FtM to have them adopted into the
  main repo.
