# WIP: ArchivesSpace Search Modifications

## About

An ArchivesSpace plugin which modifies the default PUI search behavior to
omit search results from matches to url strings where the search term(s)
contain a date like (e.g. YYYY) pattern. 

Read this [important note about omitted results](#omitted-results-note).

For example, a search of 
`united states 1949` would result in a modified search where the following
strings are added with `NOT` clauses.

```
NOT "accessions/1949"
NOT "resources/1949"
...
```

The following types are added to the search query with a `NOT` clause
```
primary_types = [
        'accessions',
        'agents/families',
        'agents/corporate_entitites',
        'agents/people',
        'archival_objects',
        'classifications',
        'digital_objects',
        'digital_object_components',
        'locations',
        'objects',
        'resources',
        'record_groups',
        'subjects',
        'top_containers'
      ]
```

### Omitted Results Note

In certain scenarios the use of this plugin can cause relevant results to be omitted.
If the date-like string a user is searching for happens to be in a primary type whose
id is that date-like string, then that result will *not* be part of the result set.

#### Example

A user searches for `jane doe 1949`. In the Jane Doe Papers, there is some correspondence
from 1949. The Jane Doe Papers happen to have an id of 1949 (`resources/1949`). None of the
results from the Jane Doe Papers will be part of the final result set.

## Installation

Install as you normally would. There are no configuration options. Add
`aspace_search_modifications` to `AppConfig[:plugins]` in your configuration.

The plugin does not have any additional dependencies so you do not need to 
run the `initialize-plugin` script.

## Compatibility

Compatible with ArchivesSpace v3.3.1 - 3.5.0.

## Core Overrides

Two core methods from the Searchable module (`/public/app/controllers/concerns/searchable.rb`)
are patched in this plugin. If you are patching these in other plugins, you will need to 
reconcile the two.

```
Searchable::set_up_advanced_search
Searchable::set_search_statement
```

## Credits

Plugin developed by Joshua Shaw [Joshua.D.Shaw@dartmouth.edu], Digital Library Technologies Group
Dartmouth Library, Dartmouth College
