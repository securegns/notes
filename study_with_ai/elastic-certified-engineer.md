================================================================================
           ELASTIC CERTIFIED ENGINEER EXAM PREPARATION GUIDE
                    COMPREHENSIVE STUDY MATERIAL
================================================================================

EXAM OVERVIEW
================================================================================
- Exam Name: Elastic Certified Engineer
- Duration: 3 hours
- Format: Hands-on, performance-based exam
- Environment: Live Elasticsearch cluster
- Passing Score: Not publicly disclosed
- Prerequisites: None (but 6-12 months hands-on experience recommended)
- Certification Validity: 2 years
- Exam Version: Based on Elasticsearch 8.x

================================================================================
                         OFFICIAL EXAM TOPICS
================================================================================

The exam covers the following major domains:
1. Data Management
2. Searching Data
3. Developing Search Applications
4. Data Processing
5. Cluster Management

================================================================================
DOMAIN 1: DATA MANAGEMENT (20-25% of exam)
================================================================================

1.1 INDEXING DOCUMENTS
------------------------------------------------------------------------------
Topics:
- Index a document using the Index API
- Update a document using the Update API
- Delete a document using the Delete API
- Bulk indexing using the Bulk API
- Reindex documents using the Reindex API

Key Concepts:
• Document structure (JSON format)
• _id field (auto-generated vs custom)
• Versioning and optimistic concurrency control
• op_type parameter (index vs create)
• Routing documents to specific shards

Commands to Master:
```
PUT /<index>/_doc/<_id>
POST /<index>/_doc
POST /<index>/_update/<_id>
DELETE /<index>/_doc/<_id>
POST /_bulk
POST /_reindex
```

Bulk API Format:
```
{ "index": { "_index": "test", "_id": "1" } }
{ "field1": "value1" }
{ "delete": { "_index": "test", "_id": "2" } }
{ "create": { "_index": "test", "_id": "3" } }
{ "field1": "value3" }
{ "update": { "_index": "test", "_id": "1" } }
{ "doc": { "field2": "value2" } }
```

Reindex API Options:
- source.index
- source.query (filter documents)
- dest.index
- dest.op_type
- dest.pipeline
- script (inline transformations)
- conflicts: proceed/abort
- size (limit documents)
- slice (parallel reindexing)

Update by Query API (EXAM CRITICAL):
```
POST my-index/_update_by_query
{
  "query": {
    "term": { "status": "pending" }
  },
  "script": {
    "source": "ctx._source.status = 'processed'; ctx._source.processed_at = params.now",
    "params": { "now": "2024-01-15" }
  }
}

// With conflicts handling and slicing for performance
POST my-index/_update_by_query?conflicts=proceed&scroll_size=1000&slices=auto
{
  "query": { "match_all": {} },
  "script": { "source": "ctx._source.version = 2" }
}
```

Delete by Query API (EXAM CRITICAL):
```
POST my-index/_delete_by_query
{
  "query": {
    "range": {
      "@timestamp": { "lt": "now-30d" }
    }
  }
}

// Async delete with conflicts handling
POST my-index/_delete_by_query?conflicts=proceed&wait_for_completion=false
{
  "query": { "term": { "status": "deleted" } }
}
```

------------------------------------------------------------------------------
1.2 DEFINING INDEX MAPPINGS
------------------------------------------------------------------------------
Topics:
- Define an explicit mapping for an index
- Define a dynamic template
- Define a custom analyzer
- Define multi-fields
- Configure field data types
- Use mapping parameters

Core Data Types:
• text - Full-text searchable content
• keyword - Exact value matching, aggregations, sorting
• long, integer, short, byte - Numeric (integer)
• double, float, half_float, scaled_float - Numeric (floating point)
• date - Date/time values
• boolean - true/false
• ip - IPv4 and IPv6 addresses
• geo_point - Latitude/longitude pairs
• geo_shape - Complex geographic shapes
• object - JSON object (flattened)
• nested - Array of objects (preserved relationships)
• flattened - Entire JSON object as single field
• join - Parent/child relationships
• binary - Base64-encoded binary
• range - Integer, float, long, double, date, ip ranges
• completion - Autocomplete suggestions
• search_as_you_type - Prefix/infix search
• token_count - Count of tokens
• dense_vector - Dense vectors for ML
• sparse_vector - Sparse vectors
• rank_feature - Numeric feature for ranking
• rank_features - Multiple ranking features
• alias - Alternative name for a field
• histogram - Pre-aggregated histogram

Mapping Parameters:
• analyzer - Specifies the analyzer for text fields
• search_analyzer - Analyzer used at search time
• normalizer - Normalizer for keyword fields
• boost - Field-level query boost (deprecated in 8.x)
• coerce - Attempt to clean up values
• copy_to - Copy field values to another field
• doc_values - Enable/disable doc values (default: true)
• dynamic - Control dynamic field mapping
• eager_global_ordinals - Pre-load global ordinals
• enabled - Enable/disable field indexing
• fielddata - Enable fielddata for text fields
• fields - Multi-fields definition
• format - Date format specification
• ignore_above - Ignore strings above length
• ignore_malformed - Ignore malformed values
• index - Enable/disable indexing
• index_options - What info to store in index
• index_phrases - Index 2-shingles for phrase queries
• index_prefixes - Index prefixes for prefix queries
• meta - Metadata about the field
• norms - Enable/disable norms (length normalization)
• null_value - Replace null with specified value
• position_increment_gap - Gap between array elements
• properties - Sub-fields for object/nested
• similarity - Scoring algorithm
• store - Store original field value
• term_vector - Store term vectors

Dynamic Mapping:
- dynamic: true (default) - New fields auto-mapped
- dynamic: false - New fields ignored (still in _source)
- dynamic: strict - Reject documents with unmapped fields
- dynamic: runtime - New fields as runtime fields

Dynamic Templates:
```
"dynamic_templates": [
  {
    "template_name": {
      "match_mapping_type": "string",
      "match": "*_text",
      "unmatch": "*_keyword",
      "path_match": "name.*",
      "path_unmatch": "name.middle.*",
      "mapping": {
        "type": "text",
        "analyzer": "english"
      }
    }
  }
]
```

Multi-fields Example:
```
"properties": {
  "title": {
    "type": "text",
    "fields": {
      "keyword": {
        "type": "keyword",
        "ignore_above": 256
      },
      "english": {
        "type": "text",
        "analyzer": "english"
      }
    }
  }
}
```

------------------------------------------------------------------------------
1.3 INDEX TEMPLATES
------------------------------------------------------------------------------
Topics:
- Create and manage index templates
- Create and manage component templates
- Configure index settings in templates
- Template priority and pattern matching

Index Template Structure:
```
PUT _index_template/my_template
{
  "index_patterns": ["logs-*"],
  "priority": 100,
  "template": {
    "settings": {
      "number_of_shards": 2,
      "number_of_replicas": 1
    },
    "mappings": {
      "properties": {}
    },
    "aliases": {
      "logs": {}
    }
  },
  "composed_of": ["component1", "component2"],
  "version": 1,
  "_meta": {
    "description": "Template for log indices"
  }
}
```

Component Templates:
```
PUT _component_template/my_component
{
  "template": {
    "mappings": {
      "properties": {
        "@timestamp": { "type": "date" }
      }
    }
  }
}
```

Template Priority:
- Higher priority values take precedence
- Component templates applied in order listed
- Index template settings override component settings

------------------------------------------------------------------------------
1.4 INDEX ALIASES
------------------------------------------------------------------------------
Topics:
- Create and manage aliases
- Filter aliases
- Routing aliases
- Write index designation

Alias Operations:
```
POST _aliases
{
  "actions": [
    { "add": { "index": "logs-2024", "alias": "logs" } },
    { "remove": { "index": "logs-2023", "alias": "logs" } },
    { "add": { 
        "index": "logs-2024", 
        "alias": "logs-errors",
        "filter": { "term": { "level": "error" } },
        "routing": "1",
        "is_write_index": true
      }
    }
  ]
}
```

Alias Use Cases:
• Zero-downtime reindexing
• Multi-index searches
• Filtered views of data
• Time-based index management
• Index abstraction layer

------------------------------------------------------------------------------
1.5 INDEX LIFECYCLE MANAGEMENT (ILM)
------------------------------------------------------------------------------
Topics:
- Define ILM policies
- Configure policy phases (hot, warm, cold, frozen, delete)
- Apply policies to indices
- Rollover configuration

ILM Phases:
1. HOT Phase:
   - rollover: Trigger based on age, size, doc count
   - set_priority: Index priority
   - shrink: Reduce shard count
   - forcemerge: Merge segments
   - readonly: Make index read-only

2. WARM Phase:
   - allocate: Move to warm nodes
   - set_priority: Lower priority
   - shrink: Reduce shards
   - forcemerge: Optimize segments
   - readonly: Read-only mode

3. COLD Phase:
   - allocate: Move to cold nodes
   - set_priority: Lowest priority
   - searchable_snapshot: Mount as searchable snapshot
   - freeze: Freeze index (deprecated)

4. FROZEN Phase:
   - searchable_snapshot: Partial mount from snapshot

5. DELETE Phase:
   - wait_for_snapshot: Wait before deletion
   - delete: Remove index

ILM Policy Example:
```
PUT _ilm/policy/my_policy
{
  "policy": {
    "phases": {
      "hot": {
        "min_age": "0ms",
        "actions": {
          "rollover": {
            "max_age": "7d",
            "max_primary_shard_size": "50gb",
            "max_docs": 100000000
          },
          "set_priority": { "priority": 100 }
        }
      },
      "warm": {
        "min_age": "30d",
        "actions": {
          "shrink": { "number_of_shards": 1 },
          "forcemerge": { "max_num_segments": 1 },
          "set_priority": { "priority": 50 }
        }
      },
      "cold": {
        "min_age": "90d",
        "actions": {
          "allocate": {
            "require": { "data": "cold" }
          },
          "set_priority": { "priority": 0 }
        }
      },
      "delete": {
        "min_age": "365d",
        "actions": {
          "delete": {}
        }
      }
    }
  }
}
```

------------------------------------------------------------------------------
1.6 DATA STREAMS
------------------------------------------------------------------------------
Topics:
- Create and manage data streams
- Configure backing indices
- Data stream naming conventions
- Timestamp field requirements

Data Stream Concepts:
• Append-only time series data
• Automatic backing index management
• Built-in ILM integration
• @timestamp field required

Create Data Stream:
```
PUT _index_template/my_data_stream_template
{
  "index_patterns": ["logs-*"],
  "data_stream": {},
  "priority": 200,
  "template": {
    "mappings": {
      "properties": {
        "@timestamp": { "type": "date" }
      }
    }
  }
}

PUT _data_stream/logs-myapp
```

Data Stream Operations:
```
GET _data_stream/logs-*
POST logs-myapp/_rollover
DELETE _data_stream/logs-myapp
```

================================================================================
DOMAIN 2: SEARCHING DATA (35-40% of exam)
================================================================================

2.1 FULL-TEXT QUERIES
------------------------------------------------------------------------------
Topics:
- match query
- match_phrase query
- match_phrase_prefix query
- multi_match query
- query_string query
- simple_query_string query
- combined_fields query
- intervals query

Match Query:
```
{
  "query": {
    "match": {
      "message": {
        "query": "quick brown fox",
        "operator": "and",
        "minimum_should_match": "75%",
        "fuzziness": "AUTO",
        "prefix_length": 2,
        "max_expansions": 50,
        "analyzer": "standard",
        "lenient": true,
        "zero_terms_query": "all"
      }
    }
  }
}
```

Match Phrase Query:
```
{
  "query": {
    "match_phrase": {
      "message": {
        "query": "quick brown fox",
        "slop": 2,
        "analyzer": "standard"
      }
    }
  }
}
```

Multi-Match Query:
```
{
  "query": {
    "multi_match": {
      "query": "quick brown fox",
      "fields": ["title^3", "content", "tags"],
      "type": "best_fields",
      "tie_breaker": 0.3,
      "operator": "and",
      "minimum_should_match": "2"
    }
  }
}
```

Multi-Match Types:
• best_fields - Default, highest score from any field
• most_fields - Sum scores from all matching fields
• cross_fields - Treat fields as one big field
• phrase - Run match_phrase on each field
• phrase_prefix - Run match_phrase_prefix on each field
• bool_prefix - Creates bool query with term queries

Query String:
```
{
  "query": {
    "query_string": {
      "query": "(content:quick OR title:brown) AND author:john*",
      "default_field": "content",
      "default_operator": "AND",
      "analyzer": "standard",
      "allow_leading_wildcard": false,
      "enable_position_increments": true,
      "fuzziness": "AUTO",
      "fuzzy_prefix_length": 2,
      "phrase_slop": 0,
      "boost": 1.0,
      "analyze_wildcard": false
    }
  }
}
```

Combined Fields Query (8.x):
```
{
  "query": {
    "combined_fields": {
      "query": "quick brown fox",
      "fields": ["title", "content", "tags"],
      "operator": "and"
    }
  }
}
```

Intervals Query (Advanced Proximity Search):
```
{
  "query": {
    "intervals": {
      "content": {
        "all_of": {
          "ordered": true,
          "max_gaps": 5,
          "intervals": [
            { "match": { "query": "elasticsearch" } },
            { "match": { "query": "search engine" } }
          ]
        }
      }
    }
  }
}

// Match any of the intervals
{
  "query": {
    "intervals": {
      "content": {
        "any_of": {
          "intervals": [
            { "match": { "query": "quick fox", "max_gaps": 1, "ordered": true } },
            { "match": { "query": "fast dog" } }
          ]
        }
      }
    }
  }
}
```

------------------------------------------------------------------------------
2.2 TERM-LEVEL QUERIES
------------------------------------------------------------------------------
Topics:
- term query
- terms query
- terms_set query
- range query
- exists query
- prefix query
- wildcard query
- regexp query
- fuzzy query
- ids query

Term Query:
```
{
  "query": {
    "term": {
      "status.keyword": {
        "value": "published",
        "boost": 1.5,
        "case_insensitive": false
      }
    }
  }
}
```

Terms Query:
```
{
  "query": {
    "terms": {
      "status": ["pending", "published"],
      "boost": 1.0
    }
  }
}
```

Terms Lookup:
```
{
  "query": {
    "terms": {
      "user.id": {
        "index": "users",
        "id": "1",
        "path": "followers"
      }
    }
  }
}
```

Range Query (Numeric):
```
{
  "query": {
    "range": {
      "age": {
        "gte": 18,
        "lte": 65,
        "boost": 2.0
      }
    }
  }
}
```

Range Query (Date):
```
{
  "query": {
    "range": {
      "date": {
        "gte": "2024-01-01",
        "lte": "now",
        "format": "yyyy-MM-dd",
        "time_zone": "+01:00"
      }
    }
  }
}
```

Range Operators:
• gt - Greater than
• gte - Greater than or equal
• lt - Less than
• lte - Less than or equal

Date Math:
• now - Current timestamp
• now-1d - Yesterday
• now-1M - One month ago
• now/d - Start of current day
• 2024-01-01||+1M/d - Jan 1 + 1 month, rounded to day

Prefix Query:
```
{
  "query": {
    "prefix": {
      "user.name": {
        "value": "john",
        "rewrite": "constant_score",
        "case_insensitive": true
      }
    }
  }
}
```

Wildcard Query:
```
{
  "query": {
    "wildcard": {
      "user.name": {
        "value": "john*son",
        "boost": 1.0,
        "rewrite": "constant_score",
        "case_insensitive": true
      }
    }
  }
}
```
Note: * matches any characters, ? matches single character

Regexp Query:
```
{
  "query": {
    "regexp": {
      "user.name": {
        "value": "john(son|ny)",
        "flags": "ALL",
        "case_insensitive": true,
        "max_determinized_states": 10000,
        "rewrite": "constant_score"
      }
    }
  }
}
```

Fuzzy Query:
```
{
  "query": {
    "fuzzy": {
      "user.name": {
        "value": "johnn",
        "fuzziness": "AUTO",
        "prefix_length": 2,
        "max_expansions": 50,
        "transpositions": true
      }
    }
  }
}
```

Fuzziness Values:
• AUTO - Based on term length (0-2: 0, 3-5: 1, >5: 2)
• 0, 1, 2 - Exact edit distance

Exists Query:
```
{
  "query": {
    "exists": {
      "field": "user.email"
    }
  }
}
```

Terms Set Query:
```
{
  "query": {
    "terms_set": {
      "required_tags": {
        "terms": ["elasticsearch", "search", "database"],
        "minimum_should_match_field": "required_matches",
        "minimum_should_match_script": {
          "source": "Math.min(params.num_terms, doc['required_matches'].value)"
        }
      }
    }
  }
}
```

IDs Query:
```
{
  "query": {
    "ids": {
      "values": ["1", "2", "3", "4"]
    }
  }
}
```

------------------------------------------------------------------------------
2.3 COMPOUND QUERIES
------------------------------------------------------------------------------
Topics:
- bool query
- boosting query
- constant_score query
- dis_max query
- function_score query

Bool Query:
```
{
  "query": {
    "bool": {
      "must": [
        { "match": { "title": "search" } }
      ],
      "must_not": [
        { "term": { "status": "draft" } }
      ],
      "should": [
        { "term": { "featured": true } },
        { "range": { "date": { "gte": "now-7d" } } }
      ],
      "filter": [
        { "term": { "category": "tech" } },
        { "range": { "price": { "lte": 100 } } }
      ],
      "minimum_should_match": 1,
      "boost": 1.0
    }
  }
}
```

Bool Clauses:
• must - Must match, contributes to score
• must_not - Must not match, no scoring
• should - Optional matching, contributes to score
• filter - Must match, no scoring (cached)

Boosting Query:
```
{
  "query": {
    "boosting": {
      "positive": {
        "match": { "content": "elasticsearch" }
      },
      "negative": {
        "term": { "type": "deprecated" }
      },
      "negative_boost": 0.5
    }
  }
}
```

Constant Score Query:
```
{
  "query": {
    "constant_score": {
      "filter": {
        "term": { "status": "active" }
      },
      "boost": 1.5
    }
  }
}
```

Dis Max Query:
```
{
  "query": {
    "dis_max": {
      "queries": [
        { "match": { "title": "elasticsearch" } },
        { "match": { "content": "elasticsearch" } }
      ],
      "tie_breaker": 0.7
    }
  }
}
```

Function Score Query:
```
{
  "query": {
    "function_score": {
      "query": { "match_all": {} },
      "functions": [
        {
          "filter": { "term": { "featured": true } },
          "weight": 2
        },
        {
          "field_value_factor": {
            "field": "popularity",
            "factor": 1.2,
            "modifier": "sqrt",
            "missing": 1
          }
        },
        {
          "gauss": {
            "date": {
              "origin": "now",
              "scale": "10d",
              "offset": "5d",
              "decay": 0.5
            }
          }
        },
        {
          "script_score": {
            "script": {
              "source": "_score * doc['boost'].value"
            }
          }
        },
        {
          "random_score": {
            "seed": 12345,
            "field": "_seq_no"
          }
        }
      ],
      "score_mode": "multiply",
      "boost_mode": "replace",
      "max_boost": 10,
      "min_score": 5
    }
  }
}
```

Score Modes:
• multiply - Multiply function scores
• sum - Add function scores
• avg - Average function scores
• first - Use first function with matching filter
• max - Maximum function score
• min - Minimum function score

Boost Modes:
• multiply - Query score * function score
• replace - Only function score
• sum - Query score + function score
• avg - Average of scores
• max - Maximum of scores
• min - Minimum of scores

------------------------------------------------------------------------------
2.4 JOINING QUERIES
------------------------------------------------------------------------------
Topics:
- nested query
- has_child query
- has_parent query
- parent_id query

Nested Query:
```
{
  "query": {
    "nested": {
      "path": "comments",
      "query": {
        "bool": {
          "must": [
            { "match": { "comments.author": "john" } },
            { "range": { "comments.votes": { "gte": 5 } } }
          ]
        }
      },
      "score_mode": "avg",
      "ignore_unmapped": false,
      "inner_hits": {
        "name": "matched_comments",
        "size": 3,
        "_source": ["comments.text"]
      }
    }
  }
}
```

Nested Score Modes:
• avg - Average score of matching nested docs
• sum - Sum of scores
• min - Minimum score
• max - Maximum score
• none - No scoring

Has Child Query:
```
{
  "query": {
    "has_child": {
      "type": "answer",
      "query": {
        "match": { "text": "elasticsearch" }
      },
      "score_mode": "max",
      "min_children": 1,
      "max_children": 5,
      "inner_hits": {}
    }
  }
}
```

Has Parent Query:
```
{
  "query": {
    "has_parent": {
      "parent_type": "question",
      "query": {
        "term": { "tags": "elasticsearch" }
      },
      "score": true,
      "inner_hits": {}
    }
  }
}
```

Parent-Child Setup:
```
PUT my_index
{
  "mappings": {
    "properties": {
      "join_field": {
        "type": "join",
        "relations": {
          "question": "answer"
        }
      }
    }
  }
}
```

------------------------------------------------------------------------------
2.5 GEO QUERIES
------------------------------------------------------------------------------
Topics:
- geo_bounding_box query
- geo_distance query
- geo_polygon query (deprecated)
- geo_shape query

Geo Distance Query:
```
{
  "query": {
    "geo_distance": {
      "distance": "200km",
      "location": {
        "lat": 40.73,
        "lon": -73.93
      },
      "distance_type": "arc",
      "validation_method": "STRICT"
    }
  }
}
```

Geo Bounding Box:
```
{
  "query": {
    "geo_bounding_box": {
      "location": {
        "top_left": {
          "lat": 42,
          "lon": -74
        },
        "bottom_right": {
          "lat": 40,
          "lon": -71
        }
      },
      "validation_method": "COERCE"
    }
  }
}
```

Geo Shape Query:
```
{
  "query": {
    "geo_shape": {
      "location": {
        "shape": {
          "type": "polygon",
          "coordinates": [
            [[0, 0], [10, 0], [10, 10], [0, 10], [0, 0]]
          ]
        },
        "relation": "within"
      }
    }
  }
}
```

Geo Relations:
• intersects - Default, shapes overlap
• within - Indexed shape within query shape
• contains - Indexed shape contains query shape
• disjoint - Shapes don't overlap

------------------------------------------------------------------------------
2.6 SPECIALIZED QUERIES
------------------------------------------------------------------------------
Topics:
- more_like_this query
- script query
- script_score query
- percolate query
- wrapper query
- pinned query
- knn query (vector search)

More Like This:
```
{
  "query": {
    "more_like_this": {
      "fields": ["title", "content"],
      "like": [
        {
          "_index": "articles",
          "_id": "1"
        },
        "elasticsearch tutorial"
      ],
      "min_term_freq": 1,
      "min_doc_freq": 1,
      "max_query_terms": 25,
      "minimum_should_match": "30%"
    }
  }
}
```

Script Query:
```
{
  "query": {
    "script": {
      "script": {
        "source": "doc['num1'].value > params.threshold",
        "params": {
          "threshold": 10
        }
      }
    }
  }
}
```

Percolate Query:
```
{
  "query": {
    "percolate": {
      "field": "query",
      "document": {
        "message": "A new elasticsearch document"
      }
    }
  }
}
```

Pinned Query:
```
{
  "query": {
    "pinned": {
      "ids": ["1", "2", "3"],
      "organic": {
        "match": { "content": "elasticsearch" }
      }
    }
  }
}
```

KNN Query (Vector Search):
```
{
  "knn": {
    "field": "image_vector",
    "query_vector": [0.1, 0.2, 0.3, ...],
    "k": 10,
    "num_candidates": 100,
    "filter": {
      "term": { "category": "images" }
    }
  }
}
```

------------------------------------------------------------------------------
2.7 SEARCH FEATURES
------------------------------------------------------------------------------
Topics:
- Pagination (from/size, search_after, scroll)
- Sorting
- Source filtering
- Highlighting
- Explain API
- Field collapsing
- Search templates
- Profile API

Pagination Options:

From/Size (up to 10,000 results):
```
{
  "from": 0,
  "size": 10,
  "query": { "match_all": {} }
}
```

Search After:
```
{
  "size": 10,
  "query": { "match_all": {} },
  "sort": [
    { "date": "desc" },
    { "_id": "asc" }
  ],
  "search_after": ["2024-01-01T00:00:00", "abc123"]
}
```

Scroll API (for processing large result sets):
```
// Initial request
POST /my-index/_search?scroll=5m
{
  "size": 1000,
  "query": { "match_all": {} }
}

// Subsequent requests using scroll_id
POST /_search/scroll
{
  "scroll": "5m",
  "scroll_id": "DXF1ZXJ5QW5kRmV0Y2g..."
}

// Clear scroll when done
DELETE /_search/scroll
{
  "scroll_id": "DXF1ZXJ5QW5kRmV0Y2g..."
}
```
Note: Scroll is deprecated for deep pagination. Use Point in Time (PIT) instead.

Point in Time (PIT) - Recommended:
```
POST /my-index/_pit?keep_alive=5m

{
  "size": 10,
  "query": { "match_all": {} },
  "pit": {
    "id": "pit_id_here",
    "keep_alive": "5m"
  },
  "sort": [{ "_shard_doc": "asc" }],
  "search_after": [123456]
}

// Close PIT when done
DELETE /_pit
{
  "id": "pit_id_here"
}
```

Sorting:
```
{
  "sort": [
    { "date": { "order": "desc", "format": "strict_date_optional_time_nanos" } },
    { "price": { "order": "asc", "missing": "_last" } },
    { "_geo_distance": {
        "location": { "lat": 40, "lon": -70 },
        "order": "asc",
        "unit": "km",
        "mode": "min"
      }
    },
    { "_script": {
        "type": "number",
        "script": { "source": "doc['field'].value * params.factor", "params": { "factor": 2 } },
        "order": "asc"
      }
    },
    "_score"
  ]
}
```

Source Filtering:
```
// Disable _source entirely
{ "_source": false }

// Include specific fields only
{ "_source": ["field1", "field2"] }

// Include/exclude patterns
{
  "_source": {
    "includes": ["obj.*"],
    "excludes": ["*.secret"]
  }
}

// Using fields parameter (recommended for 8.x)
{
  "_source": false,
  "fields": ["date", "user.name"]
}
```

Highlighting:
```
{
  "query": { "match": { "content": "elasticsearch" } },
  "highlight": {
    "pre_tags": ["<em>"],
    "post_tags": ["</em>"],
    "fields": {
      "content": {
        "type": "unified",
        "fragment_size": 150,
        "number_of_fragments": 3,
        "no_match_size": 150
      },
      "title": {
        "type": "plain",
        "number_of_fragments": 0
      }
    },
    "encoder": "html",
    "require_field_match": true
  }
}
```

Highlighter Types:
• unified - Default, best for most cases
• plain - Standard highlighter
• fvh - Fast vector highlighter (requires term_vector)

Field Collapsing:
```
{
  "query": { "match_all": {} },
  "collapse": {
    "field": "user.id",
    "inner_hits": {
      "name": "most_recent",
      "size": 3,
      "sort": [{ "date": "desc" }]
    }
  }
}
```

Search Templates:
```
PUT _scripts/my_template
{
  "script": {
    "lang": "mustache",
    "source": {
      "query": {
        "match": {
          "{{field}}": "{{value}}"
        }
      },
      "size": "{{size}}{{^size}}10{{/size}}"
    }
  }
}

POST my-index/_search/template
{
  "id": "my_template",
  "params": {
    "field": "title",
    "value": "elasticsearch"
  }
}
```

Profile API:
```
{
  "profile": true,
  "query": { "match": { "content": "elasticsearch" } }
}
```

Explain API:
```
GET /my-index/_explain/1
{
  "query": { "match": { "content": "elasticsearch" } }
}
```

Async Search (8.x - for long-running queries):
```
// Submit async search
POST /my-index/_async_search?wait_for_completion_timeout=1s
{
  "size": 0,
  "aggs": {
    "expensive_agg": { "terms": { "field": "user.keyword", "size": 10000 } }
  }
}

// Get async search results
GET /_async_search/<async_search_id>

// Get status without results
GET /_async_search/status/<async_search_id>

// Delete async search
DELETE /_async_search/<async_search_id>
```

Multi-Search API (_msearch):
```
POST /_msearch
{"index": "index1"}
{"query": {"match": {"title": "elasticsearch"}}}
{"index": "index2"}
{"query": {"match": {"content": "search"}}}
{"index": "index1,index2"}
{"query": {"match_all": {}}, "size": 5}
```

Field Capabilities API:
```
GET /my-index/_field_caps?fields=title,content,*date*

// Across multiple indices
GET /logs-*/_field_caps?fields=message,@timestamp
{
  "index_filter": {
    "range": { "@timestamp": { "gte": "2024-01-01" } }
  }
}
```

Validate Query API:
```
GET /my-index/_validate/query?explain=true
{
  "query": { "match": { "content": "elasticsearch" } }
}
```

================================================================================
DOMAIN 3: DEVELOPING SEARCH APPLICATIONS (15-20% of exam)
================================================================================

3.1 TEXT ANALYSIS
------------------------------------------------------------------------------
Topics:
- Built-in analyzers
- Custom analyzers
- Character filters
- Tokenizers
- Token filters
- Analyze API

Built-in Analyzers:
• standard - Default, grammar-based tokenization
• simple - Lowercase, splits on non-letters
• whitespace - Splits on whitespace only
• stop - Like simple + stop words removal
• keyword - No-op, entire input as single token
• pattern - Regex-based splitting
• language analyzers - english, french, german, etc.
• fingerprint - Deduplication and sorting

Custom Analyzer Structure:
```
PUT my-index
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_custom_analyzer": {
          "type": "custom",
          "char_filter": ["html_strip", "my_char_filter"],
          "tokenizer": "standard",
          "filter": ["lowercase", "my_synonym_filter", "my_stemmer"]
        }
      },
      "char_filter": {
        "my_char_filter": {
          "type": "mapping",
          "mappings": ["& => and", "| => or"]
        }
      },
      "tokenizer": {
        "my_tokenizer": {
          "type": "pattern",
          "pattern": "[\\W&&[^-]]+"
        }
      },
      "filter": {
        "my_synonym_filter": {
          "type": "synonym",
          "synonyms": ["quick,fast,speedy"]
        },
        "my_stemmer": {
          "type": "stemmer",
          "language": "english"
        }
      }
    }
  }
}
```

Character Filters:
• html_strip - Remove HTML tags
• mapping - Replace characters/strings
• pattern_replace - Regex replacement

Tokenizers:
• standard - Grammar-based word tokenization
• letter - Non-letter character splits
• lowercase - Like letter + lowercase
• whitespace - Whitespace splits
• uax_url_email - Like standard, preserves URLs/emails
• classic - Grammar-based, English-optimized
• thai - Thai text segmentation
• ngram - N-gram tokenization
• edge_ngram - Edge n-gram tokenization
• keyword - No tokenization
• pattern - Regex-based tokenization
• char_group - Split on character groups
• simple_pattern - Simple pattern matching
• simple_pattern_split - Pattern-based splitting
• path_hierarchy - File path tokenization

Common Token Filters:
• lowercase - Convert to lowercase
• uppercase - Convert to uppercase
• stop - Remove stop words
• stemmer - Language-specific stemming
• snowball - Snowball stemming
• porter_stem - Porter stemming
• kstem - KStem stemming
• hunspell - Hunspell stemming
• synonym - Synonym expansion
• synonym_graph - Graph-aware synonyms
• word_delimiter - Split on word delimiters
• word_delimiter_graph - Graph-aware word delimiter
• asciifolding - Convert to ASCII equivalents
• ngram - Generate n-grams
• edge_ngram - Generate edge n-grams
• shingle - Word shingles (word n-grams)
• truncate - Truncate tokens to length
• trim - Remove whitespace
• limit - Limit token count
• unique - Remove duplicates
• reverse - Reverse tokens
• elision - Remove elisions
• pattern_capture - Capture regex groups
• pattern_replace - Regex replacement
• fingerprint - Create fingerprint
• phonetic - Phonetic encoding (plugin)

Analyze API:
```
POST _analyze
{
  "analyzer": "standard",
  "text": "The quick brown fox"
}

POST my-index/_analyze
{
  "field": "content",
  "text": "The quick brown fox"
}

POST _analyze
{
  "tokenizer": "standard",
  "filter": ["lowercase", "asciifolding"],
  "text": "Élastic Search"
}
```

N-gram Configuration:
```
"tokenizer": {
  "my_ngram": {
    "type": "ngram",
    "min_gram": 2,
    "max_gram": 3,
    "token_chars": ["letter", "digit"]
  }
}
```

Edge N-gram:
```
"tokenizer": {
  "my_edge_ngram": {
    "type": "edge_ngram",
    "min_gram": 1,
    "max_gram": 10,
    "token_chars": ["letter", "digit"]
  }
}
```

Synonym Filter:
```
"filter": {
  "my_synonyms": {
    "type": "synonym",
    "synonyms_path": "analysis/synonyms.txt",
    "synonyms": [
      "quick, fast, speedy => rapid",
      "laptop, notebook, computer"
    ],
    "expand": true,
    "lenient": false
  }
}
```

------------------------------------------------------------------------------
3.2 NORMALIZERS
------------------------------------------------------------------------------
Topics:
- Custom normalizers for keyword fields
- Character and token filters for normalizers

Normalizer Definition:
```
PUT my-index
{
  "settings": {
    "analysis": {
      "normalizer": {
        "my_normalizer": {
          "type": "custom",
          "char_filter": [],
          "filter": ["lowercase", "asciifolding"]
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "status": {
        "type": "keyword",
        "normalizer": "my_normalizer"
      }
    }
  }
}
```

------------------------------------------------------------------------------
3.3 SUGGESTERS
------------------------------------------------------------------------------
Topics:
- Term suggester
- Phrase suggester
- Completion suggester

Term Suggester:
```
POST my-index/_search
{
  "suggest": {
    "my-suggest": {
      "text": "elasticsearsh",
      "term": {
        "field": "content",
        "suggest_mode": "popular",
        "min_word_length": 4,
        "prefix_length": 1,
        "max_edits": 2,
        "max_term_freq": 0.01,
        "min_doc_freq": 1
      }
    }
  }
}
```

Phrase Suggester:
```
POST my-index/_search
{
  "suggest": {
    "my-suggest": {
      "text": "the quikc brown fxo",
      "phrase": {
        "field": "content.trigram",
        "size": 3,
        "gram_size": 3,
        "confidence": 1,
        "max_errors": 2,
        "highlight": {
          "pre_tag": "<em>",
          "post_tag": "</em>"
        },
        "collate": {
          "query": {
            "source": {
              "match": { "content": "{{suggestion}}" }
            }
          },
          "prune": true
        }
      }
    }
  }
}
```

Completion Suggester (Autocomplete):
```
PUT my-index
{
  "mappings": {
    "properties": {
      "suggest": {
        "type": "completion",
        "analyzer": "simple",
        "preserve_separators": true,
        "preserve_position_increments": true,
        "max_input_length": 50
      }
    }
  }
}

POST my-index/_doc
{
  "suggest": {
    "input": ["Elasticsearch", "Search Engine"],
    "weight": 10
  }
}

POST my-index/_search
{
  "suggest": {
    "song-suggest": {
      "prefix": "ela",
      "completion": {
        "field": "suggest",
        "size": 5,
        "skip_duplicates": true,
        "fuzzy": {
          "fuzziness": "AUTO"
        }
      }
    }
  }
}
```

Context Suggester:
```
PUT my-index
{
  "mappings": {
    "properties": {
      "suggest": {
        "type": "completion",
        "contexts": [
          {
            "name": "category",
            "type": "category",
            "path": "category"
          },
          {
            "name": "location",
            "type": "geo",
            "precision": 4
          }
        ]
      }
    }
  }
}

POST my-index/_search
{
  "suggest": {
    "my-suggest": {
      "prefix": "ela",
      "completion": {
        "field": "suggest",
        "contexts": {
          "category": ["tech"],
          "location": {
            "lat": 40.73,
            "lon": -73.93
          }
        }
      }
    }
  }
}
```

------------------------------------------------------------------------------
3.4 RUNTIME FIELDS
------------------------------------------------------------------------------
Topics:
- Define runtime fields in mappings
- Define runtime fields at query time
- Painless scripting for runtime fields

Runtime Field in Mapping:
```
PUT my-index
{
  "mappings": {
    "runtime": {
      "full_name": {
        "type": "keyword",
        "script": {
          "source": "emit(doc['first_name'].value + ' ' + doc['last_name'].value)"
        }
      },
      "day_of_week": {
        "type": "keyword",
        "script": {
          "source": "emit(doc['@timestamp'].value.dayOfWeekEnum.getDisplayName(TextStyle.FULL, Locale.ROOT))"
        }
      }
    }
  }
}
```

Runtime Field at Query Time:
```
GET my-index/_search
{
  "runtime_mappings": {
    "price_with_tax": {
      "type": "double",
      "script": {
        "source": "emit(doc['price'].value * 1.1)"
      }
    }
  },
  "query": {
    "range": {
      "price_with_tax": { "gte": 100 }
    }
  },
  "fields": ["price_with_tax"]
}
```

Runtime Field Types:
• boolean
• date
• double
• geo_point
• ip
• keyword
• long
• lookup (cross-index lookups)

================================================================================
DOMAIN 4: DATA PROCESSING (15-20% of exam)
================================================================================

4.1 INGEST PIPELINES
------------------------------------------------------------------------------
Topics:
- Create and manage ingest pipelines
- Simulate pipeline processing
- Common processors
- Conditional processing
- Pipeline chaining

Pipeline Structure:
```
PUT _ingest/pipeline/my_pipeline
{
  "description": "Pipeline description",
  "version": 1,
  "processors": [
    {
      "set": {
        "field": "ingested_at",
        "value": "{{{_ingest.timestamp}}}"
      }
    },
    {
      "rename": {
        "field": "old_name",
        "target_field": "new_name",
        "ignore_missing": true
      }
    }
  ],
  "on_failure": [
    {
      "set": {
        "field": "error",
        "value": "{{ _ingest.on_failure_message }}"
      }
    }
  ]
}
```

Common Processors:

Set Processor:
```
{
  "set": {
    "field": "my_field",
    "value": "my_value",
    "override": true,
    "ignore_empty_value": false,
    "if": "ctx.some_field != null"
  }
}
```

Remove Processor:
```
{
  "remove": {
    "field": ["field1", "field2"],
    "ignore_missing": true
  }
}
```

Rename Processor:
```
{
  "rename": {
    "field": "old_field",
    "target_field": "new_field",
    "ignore_missing": false
  }
}
```

Convert Processor:
```
{
  "convert": {
    "field": "price",
    "type": "float",
    "target_field": "price_float",
    "ignore_missing": true
  }
}
```
Types: integer, long, float, double, string, boolean, auto

Grok Processor:
```
{
  "grok": {
    "field": "message",
    "patterns": [
      "%{IP:client} %{WORD:method} %{URIPATHPARAM:request} %{NUMBER:bytes:int} %{NUMBER:duration:float}"
    ],
    "pattern_definitions": {
      "CUSTOM_PATTERN": "custom regex"
    },
    "ignore_missing": false,
    "ignore_failure": false
  }
}
```

Common Grok Patterns:
• %{IP:field} - IP address
• %{NUMBER:field:int} - Number with type conversion
• %{WORD:field} - Single word
• %{GREEDYDATA:field} - Match everything
• %{TIMESTAMP_ISO8601:field} - ISO timestamp
• %{COMBINEDAPACHELOG} - Apache combined log
• %{SYSLOGBASE} - Syslog base
• %{LOGLEVEL:field} - Log level

Dissect Processor:
```
{
  "dissect": {
    "field": "message",
    "pattern": "%{client} %{method} %{request} %{bytes} %{duration}",
    "append_separator": " "
  }
}
```

Date Processor:
```
{
  "date": {
    "field": "date_string",
    "target_field": "@timestamp",
    "formats": ["dd/MMM/yyyy:HH:mm:ss Z", "ISO8601"],
    "timezone": "UTC"
  }
}
```

JSON Processor:
```
{
  "json": {
    "field": "json_string",
    "target_field": "parsed_json",
    "add_to_root": false
  }
}
```

Split Processor:
```
{
  "split": {
    "field": "tags",
    "separator": ",",
    "target_field": "tag_array",
    "preserve_trailing": false
  }
}
```

Join Processor:
```
{
  "join": {
    "field": "tag_array",
    "separator": ", ",
    "target_field": "tags_string"
  }
}
```

Append Processor:
```
{
  "append": {
    "field": "tags",
    "value": ["new_tag"],
    "allow_duplicates": false
  }
}
```

Trim Processor:
```
{
  "trim": {
    "field": "message",
    "target_field": "trimmed_message"
  }
}
```

Lowercase/Uppercase Processor:
```
{
  "lowercase": {
    "field": "status"
  }
},
{
  "uppercase": {
    "field": "code"
  }
}
```

GeoIP Processor:
```
{
  "geoip": {
    "field": "ip",
    "target_field": "geo",
    "database_file": "GeoLite2-City.mmdb",
    "properties": ["continent_name", "country_name", "city_name", "location"]
  }
}
```

User Agent Processor:
```
{
  "user_agent": {
    "field": "user_agent",
    "target_field": "user_agent_parsed",
    "ignore_missing": true
  }
}
```

Script Processor:
```
{
  "script": {
    "lang": "painless",
    "source": """
      ctx.fullName = ctx.firstName + ' ' + ctx.lastName;
      ctx.age = (int)((System.currentTimeMillis() - ctx.birthDate) / (365.25 * 24 * 60 * 60 * 1000));
    """,
    "params": {
      "multiplier": 2
    }
  }
}
```

Pipeline Processor (Chaining):
```
{
  "pipeline": {
    "name": "other_pipeline",
    "ignore_missing_pipeline": false
  }
}
```

Drop Processor:
```
{
  "drop": {
    "if": "ctx.level == 'DEBUG'"
  }
}
```

Foreach Processor:
```
{
  "foreach": {
    "field": "items",
    "processor": {
      "uppercase": {
        "field": "_ingest._value.name"
      }
    }
  }
}
```

Conditional Processing:
```
{
  "set": {
    "if": "ctx.status == 'error'",
    "field": "priority",
    "value": "high"
  }
}
```

Simulate Pipeline:
```
POST _ingest/pipeline/_simulate
{
  "pipeline": {
    "processors": [
      { "set": { "field": "test", "value": "value" } }
    ]
  },
  "docs": [
    { "_source": { "message": "test" } }
  ]
}

POST _ingest/pipeline/my_pipeline/_simulate
{
  "docs": [
    { "_source": { "message": "test" } }
  ]
}
```

------------------------------------------------------------------------------
4.2 ENRICH PROCESSORS
------------------------------------------------------------------------------
Topics:
- Create enrich policies
- Execute enrich policies
- Use enrich processors in pipelines

Enrich Policy:
```
PUT _enrich/policy/users_policy
{
  "match": {
    "indices": "users",
    "match_field": "email",
    "enrich_fields": ["first_name", "last_name", "department"]
  }
}

POST _enrich/policy/users_policy/_execute
```

Enrich Processor:
```
{
  "enrich": {
    "policy_name": "users_policy",
    "field": "user_email",
    "target_field": "user",
    "max_matches": 1,
    "shape_relation": "INTERSECTS"
  }
}
```

Geo Match Enrich:
```
PUT _enrich/policy/geo_policy
{
  "geo_match": {
    "indices": "regions",
    "match_field": "location",
    "enrich_fields": ["region_name", "country"]
  }
}
```

------------------------------------------------------------------------------
4.2.1 TRANSFORMS (Exam Important)
------------------------------------------------------------------------------
Topics:
- Create and manage transforms
- Pivot transforms for aggregations
- Latest transforms for current state

Pivot Transform:
```
PUT _transform/sales_summary
{
  "source": {
    "index": ["sales-*"],
    "query": {
      "range": { "@timestamp": { "gte": "now-1y" } }
    }
  },
  "pivot": {
    "group_by": {
      "product": { "terms": { "field": "product.keyword" } },
      "month": { "date_histogram": { "field": "@timestamp", "calendar_interval": "month" } }
    },
    "aggregations": {
      "total_sales": { "sum": { "field": "amount" } },
      "avg_price": { "avg": { "field": "price" } },
      "transaction_count": { "value_count": { "field": "_id" } }
    }
  },
  "dest": {
    "index": "sales_summary"
  },
  "frequency": "1h",
  "sync": {
    "time": {
      "field": "@timestamp",
      "delay": "60s"
    }
  }
}
```

Latest Transform:
```
PUT _transform/device_latest_status
{
  "source": { "index": "device-events-*" },
  "latest": {
    "unique_key": ["device_id"],
    "sort": "@timestamp"
  },
  "dest": { "index": "device-current-status" }
}
```

Transform Management:
```
POST _transform/sales_summary/_start
POST _transform/sales_summary/_stop
GET _transform/sales_summary/_stats
GET _transform/_preview
DELETE _transform/sales_summary
```

------------------------------------------------------------------------------
4.3 PAINLESS SCRIPTING
------------------------------------------------------------------------------
Topics:
- Script contexts
- Doc values access
- Params
- Stored scripts
- Scripted fields

Script Contexts:
• filter - Boolean filter scripts
• score - Scoring scripts
• update - Update scripts
• ingest - Ingest processor scripts
• aggregation - Aggregation scripts
• field - Runtime field scripts
• sort - Sort scripts

Accessing Document Values:
```
// Doc values (most efficient for sorting/aggregations)
doc['field'].value
doc['field'].values
doc['field'].size()
doc.containsKey('field')

// Source access (in update/ingest contexts)
ctx._source.field
ctx._source['field']

// Params
params.param_name
```

Stored Scripts:
```
PUT _scripts/my_script
{
  "script": {
    "lang": "painless",
    "source": "doc['field'].value * params.factor"
  }
}

GET my-index/_search
{
  "query": {
    "script_score": {
      "query": { "match_all": {} },
      "script": {
        "id": "my_script",
        "params": { "factor": 2 }
      }
    }
  }
}
```

Common Painless Operations:
```
// String operations
def str = doc['field'].value;
str.toLowerCase()
str.toUpperCase()
str.substring(0, 5)
str.contains("search")
str.replace("old", "new")
str.split(",")
str.length()

// Math operations
Math.max(a, b)
Math.min(a, b)
Math.abs(value)
Math.round(value)
Math.pow(base, exp)
Math.sqrt(value)

// Date operations
doc['date'].value.toInstant().toEpochMilli()
doc['date'].value.getYear()
doc['date'].value.getMonthValue()
doc['date'].value.getDayOfMonth()
doc['date'].value.getDayOfWeek()
ChronoUnit.DAYS.between(date1, date2)

// Collections
def list = new ArrayList();
list.add(item);
list.size();
list.contains(item);
list.indexOf(item);
list.remove(index);

def map = new HashMap();
map.put(key, value);
map.get(key);
map.containsKey(key);

// Conditionals
def result = condition ? valueIfTrue : valueIfFalse;

// Null handling
def value = doc['field'].size() > 0 ? doc['field'].value : 'default';
```

================================================================================
DOMAIN 5: CLUSTER MANAGEMENT (15-20% of exam)
================================================================================

5.1 CLUSTER CONFIGURATION
------------------------------------------------------------------------------
Topics:
- Node roles and configuration
- Cluster settings (persistent/transient)
- Discovery and cluster formation
- Shard allocation

Node Roles:
• master - Cluster management, master-eligible
• data - Data storage and search
• data_content - Content tier data
• data_hot - Hot tier data
• data_warm - Warm tier data
• data_cold - Cold tier data
• data_frozen - Frozen tier data
• ingest - Ingest pipeline processing
• ml - Machine learning
• remote_cluster_client - Cross-cluster operations
• transform - Transform processing
• voting_only - Voting but not master-eligible
• coordinating - No special role (routing only)

Node Configuration (elasticsearch.yml):
```
node.name: node-1
node.roles: [master, data_hot, ingest]

path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch

network.host: 0.0.0.0
http.port: 9200
transport.port: 9300

discovery.seed_hosts: ["host1", "host2", "host3"]
cluster.initial_master_nodes: ["node-1", "node-2", "node-3"]
cluster.name: my-cluster
```

Cluster Settings:
```
PUT _cluster/settings
{
  "persistent": {
    "cluster.routing.allocation.enable": "all",
    "indices.recovery.max_bytes_per_sec": "100mb"
  },
  "transient": {
    "cluster.routing.allocation.enable": "primaries"
  }
}

GET _cluster/settings?include_defaults=true
```

Allocation Settings:
```
{
  "cluster.routing.allocation.enable": "all|primaries|new_primaries|none",
  "cluster.routing.allocation.node_concurrent_recoveries": 2,
  "cluster.routing.allocation.node_initial_primaries_recoveries": 4,
  "cluster.routing.allocation.same_shard.host": true,
  "cluster.routing.allocation.total_shards_per_node": 1000,
  "cluster.routing.rebalance.enable": "all|primaries|replicas|none",
  "cluster.routing.allocation.disk.threshold_enabled": true,
  "cluster.routing.allocation.disk.watermark.low": "85%",
  "cluster.routing.allocation.disk.watermark.high": "90%",
  "cluster.routing.allocation.disk.watermark.flood_stage": "95%"
}
```

Allocation Filtering:
```
PUT my-index/_settings
{
  "index.routing.allocation.include.rack": "rack1,rack2",
  "index.routing.allocation.exclude.name": "node-3",
  "index.routing.allocation.require.zone": "zone1"
}
```

------------------------------------------------------------------------------
5.2 SHARD MANAGEMENT
------------------------------------------------------------------------------
Topics:
- Primary and replica shards
- Shard allocation awareness
- Forced awareness
- Shard sizing best practices

Shard Allocation Awareness:
```
# elasticsearch.yml
node.attr.rack: rack1
node.attr.zone: zone1

# Cluster settings
PUT _cluster/settings
{
  "persistent": {
    "cluster.routing.allocation.awareness.attributes": "rack,zone"
  }
}
```

Forced Awareness:
```
PUT _cluster/settings
{
  "persistent": {
    "cluster.routing.allocation.awareness.attributes": "zone",
    "cluster.routing.allocation.awareness.force.zone.values": "zone1,zone2"
  }
}
```

Index Shard Settings:
```
PUT my-index
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1,
    "index.routing.allocation.total_shards_per_node": 2
  }
}
```

Shrink Index:
```
PUT my-index/_settings
{
  "settings": {
    "index.routing.allocation.require._name": "node-1",
    "index.blocks.write": true
  }
}

POST my-index/_shrink/shrunk-index
{
  "settings": {
    "index.number_of_shards": 1,
    "index.number_of_replicas": 1,
    "index.codec": "best_compression"
  }
}
```

Split Index:
```
PUT my-index/_settings
{
  "settings": {
    "index.blocks.write": true
  }
}

POST my-index/_split/split-index
{
  "settings": {
    "index.number_of_shards": 6
  }
}
```

Shard Sizing Guidelines:
• Target 10-50GB per shard (sweet spot: 20-40GB)
• Avoid very small shards (<1GB) - causes overhead
• Avoid very large shards (>50GB) - slow recovery
• Aim for ~20 shards per GB of heap (max)
• Consider search/indexing workload balance
• Plan for future growth with rollover
• Use _forcemerge for read-only indices
• Rule of thumb: total shards = (data size / 30GB) * (1 + replicas)

------------------------------------------------------------------------------
5.3 SNAPSHOTS AND RESTORE
------------------------------------------------------------------------------
Topics:
- Register snapshot repositories
- Create and manage snapshots
- Restore indices from snapshots
- Snapshot lifecycle management (SLM)

Repository Registration:
```
# Shared filesystem
PUT _snapshot/my_fs_repo
{
  "type": "fs",
  "settings": {
    "location": "/mount/backups/my_repo",
    "compress": true,
    "chunk_size": "100mb",
    "max_restore_bytes_per_sec": "100mb",
    "max_snapshot_bytes_per_sec": "50mb"
  }
}

# S3
PUT _snapshot/my_s3_repo
{
  "type": "s3",
  "settings": {
    "bucket": "my-bucket",
    "region": "us-east-1",
    "base_path": "snapshots",
    "compress": true
  }
}

# Azure
PUT _snapshot/my_azure_repo
{
  "type": "azure",
  "settings": {
    "container": "backup-container",
    "base_path": "snapshots"
  }
}

# GCS
PUT _snapshot/my_gcs_repo
{
  "type": "gcs",
  "settings": {
    "bucket": "my-bucket",
    "base_path": "snapshots"
  }
}
```

Create Snapshot:
```
PUT _snapshot/my_repo/snapshot_1?wait_for_completion=true
{
  "indices": "index_1,index_2",
  "ignore_unavailable": true,
  "include_global_state": false,
  "partial": false,
  "metadata": {
    "taken_by": "admin",
    "reason": "weekly backup"
  }
}
```

List Snapshots:
```
GET _snapshot/my_repo/_all
GET _snapshot/my_repo/snapshot_1
GET _snapshot/my_repo/snapshot_*
GET _snapshot/_status
```

Restore Snapshot:
```
POST _snapshot/my_repo/snapshot_1/_restore
{
  "indices": "index_1,index_2",
  "ignore_unavailable": true,
  "include_global_state": false,
  "rename_pattern": "(.+)",
  "rename_replacement": "restored_$1",
  "index_settings": {
    "index.number_of_replicas": 0
  },
  "ignore_index_settings": [
    "index.refresh_interval"
  ]
}
```

Delete Snapshot:
```
DELETE _snapshot/my_repo/snapshot_1
```

Snapshot Lifecycle Management (SLM):
```
PUT _slm/policy/daily-snapshots
{
  "schedule": "0 30 1 * * ?",
  "name": "<daily-snap-{now/d}>",
  "repository": "my_repo",
  "config": {
    "indices": ["*"],
    "ignore_unavailable": false,
    "include_global_state": false
  },
  "retention": {
    "expire_after": "30d",
    "min_count": 5,
    "max_count": 50
  }
}

POST _slm/policy/daily-snapshots/_execute
GET _slm/policy/daily-snapshots
GET _slm/stats
```

------------------------------------------------------------------------------
5.4 CLUSTER MONITORING
------------------------------------------------------------------------------
Topics:
- Cluster health API
- Node stats and info
- Index stats
- Task management
- Cat APIs

Cluster Health:
```
GET _cluster/health
GET _cluster/health/my-index
GET _cluster/health?level=indices
GET _cluster/health?level=shards
GET _cluster/health?wait_for_status=yellow&timeout=30s
```

Health Status:
• green - All shards allocated
• yellow - All primaries allocated, some replicas not
• red - Some primary shards not allocated

Cluster State:
```
GET _cluster/state
GET _cluster/state/metadata,routing_table
GET _cluster/state/_all/my-index
```

Cluster Stats:
```
GET _cluster/stats
GET _cluster/stats/nodes/node-1,node-2
```

Node Info:
```
GET _nodes
GET _nodes/node-1
GET _nodes/_local
GET _nodes/_master
GET _nodes/stats
GET _nodes/hot_threads
GET _nodes/node-1/stats/jvm,os
```

Node Stats Categories:
• indices - Index statistics
• os - Operating system stats
• process - Process stats
• jvm - JVM stats
• thread_pool - Thread pool stats
• fs - File system stats
• transport - Transport layer stats
• http - HTTP stats
• breakers - Circuit breaker stats
• script - Script stats
• discovery - Discovery stats
• ingest - Ingest stats

Index Stats:
```
GET my-index/_stats
GET my-index/_stats/docs,store
GET _stats
GET _all/_stats
```

Task Management:
```
GET _tasks
GET _tasks?detailed=true
GET _tasks?nodes=node-1
GET _tasks?actions=*search*
GET _tasks/task_id
POST _tasks/task_id/_cancel
```

Cat APIs:
```
GET _cat/health?v
GET _cat/nodes?v
GET _cat/indices?v
GET _cat/shards?v
GET _cat/allocation?v
GET _cat/master?v
GET _cat/recovery?v
GET _cat/thread_pool?v
GET _cat/pending_tasks?v
GET _cat/aliases?v
GET _cat/count?v
GET _cat/segments?v
GET _cat/templates?v
GET _cat/plugins?v
GET _cat/tasks?v
```

Cat API Parameters:
• v - Verbose (column headers)
• help - Show available columns
• h=col1,col2 - Select columns
• s=col:desc - Sort by column
• format=json - Output format

Allocation Explanation:
```
GET _cluster/allocation/explain
{
  "index": "my-index",
  "shard": 0,
  "primary": true
}
```

------------------------------------------------------------------------------
5.5 CROSS-CLUSTER OPERATIONS
------------------------------------------------------------------------------
Topics:
- Cross-cluster search (CCS)
- Cross-cluster replication (CCR)
- Remote cluster configuration

Remote Cluster Configuration:
```
PUT _cluster/settings
{
  "persistent": {
    "cluster.remote.cluster_one": {
      "seeds": ["remote-host-1:9300", "remote-host-2:9300"],
      "transport.compress": true,
      "skip_unavailable": true
    }
  }
}
```

Cross-Cluster Search:
```
GET cluster_one:my-index,cluster_two:my-index/_search
{
  "query": { "match_all": {} }
}
```

Cross-Cluster Replication:
```
# On follower cluster
PUT /follower-index/_ccr/follow
{
  "remote_cluster": "leader-cluster",
  "leader_index": "leader-index"
}

# Auto-follow pattern
PUT _ccr/auto_follow/my_pattern
{
  "remote_cluster": "leader-cluster",
  "leader_index_patterns": ["logs-*"],
  "follow_index_pattern": "{{leader_index}}-replicated"
}
```

================================================================================
                         AGGREGATIONS REFERENCE
================================================================================

BUCKET AGGREGATIONS
------------------------------------------------------------------------------
Terms Aggregation:
```
{
  "aggs": {
    "genres": {
      "terms": {
        "field": "genre.keyword",
        "size": 10,
        "min_doc_count": 1,
        "shard_min_doc_count": 0,
        "show_term_doc_count_error": true,
        "order": { "_count": "desc" },
        "missing": "Unknown",
        "include": ".*sport.*",
        "exclude": ["hockey", "golf"]
      }
    }
  }
}
```

Date Histogram:
```
{
  "aggs": {
    "sales_over_time": {
      "date_histogram": {
        "field": "@timestamp",
        "calendar_interval": "month",
        "fixed_interval": "30d",
        "format": "yyyy-MM-dd",
        "time_zone": "+01:00",
        "offset": "+6h",
        "min_doc_count": 0,
        "extended_bounds": {
          "min": "2024-01-01",
          "max": "2024-12-31"
        }
      }
    }
  }
}
```

Histogram:
```
{
  "aggs": {
    "prices": {
      "histogram": {
        "field": "price",
        "interval": 50,
        "min_doc_count": 1,
        "extended_bounds": { "min": 0, "max": 500 }
      }
    }
  }
}
```

Range Aggregation:
```
{
  "aggs": {
    "price_ranges": {
      "range": {
        "field": "price",
        "keyed": true,
        "ranges": [
          { "key": "cheap", "to": 100 },
          { "key": "moderate", "from": 100, "to": 500 },
          { "key": "expensive", "from": 500 }
        ]
      }
    }
  }
}
```

Date Range:
```
{
  "aggs": {
    "date_ranges": {
      "date_range": {
        "field": "@timestamp",
        "format": "MM-yyyy",
        "ranges": [
          { "to": "now-10M/M" },
          { "from": "now-10M/M" }
        ]
      }
    }
  }
}
```

Filter Aggregation:
```
{
  "aggs": {
    "errors": {
      "filter": { "term": { "level": "error" } },
      "aggs": {
        "avg_response": { "avg": { "field": "response_time" } }
      }
    }
  }
}
```

Filters Aggregation:
```
{
  "aggs": {
    "messages": {
      "filters": {
        "other_bucket_key": "other",
        "filters": {
          "errors": { "match": { "message": "error" } },
          "warnings": { "match": { "message": "warning" } }
        }
      }
    }
  }
}
```

Nested Aggregation:
```
{
  "aggs": {
    "comments": {
      "nested": { "path": "comments" },
      "aggs": {
        "top_authors": {
          "terms": { "field": "comments.author.keyword" }
        }
      }
    }
  }
}
```

Reverse Nested:
```
{
  "aggs": {
    "comments": {
      "nested": { "path": "comments" },
      "aggs": {
        "top_authors": {
          "terms": { "field": "comments.author.keyword" },
          "aggs": {
            "parent_docs": {
              "reverse_nested": {},
              "aggs": {
                "top_categories": {
                  "terms": { "field": "category.keyword" }
                }
              }
            }
          }
        }
      }
    }
  }
}
```

Composite Aggregation:
```
{
  "aggs": {
    "my_buckets": {
      "composite": {
        "size": 100,
        "sources": [
          { "date": { "date_histogram": { "field": "@timestamp", "calendar_interval": "day" } } },
          { "product": { "terms": { "field": "product.keyword" } } }
        ],
        "after": { "date": 1609459200000, "product": "widget" }
      }
    }
  }
}
```

Geo Distance:
```
{
  "aggs": {
    "rings_around_amsterdam": {
      "geo_distance": {
        "field": "location",
        "origin": { "lat": 52.37, "lon": 4.89 },
        "unit": "km",
        "ranges": [
          { "to": 100 },
          { "from": 100, "to": 300 },
          { "from": 300 }
        ]
      }
    }
  }
}
```

METRIC AGGREGATIONS
------------------------------------------------------------------------------
Basic Metrics:
```
{
  "aggs": {
    "avg_price": { "avg": { "field": "price", "missing": 0 } },
    "sum_quantity": { "sum": { "field": "quantity" } },
    "min_price": { "min": { "field": "price" } },
    "max_price": { "max": { "field": "price" } },
    "count": { "value_count": { "field": "price" } },
    "cardinality": { 
      "cardinality": { 
        "field": "user.id", 
        "precision_threshold": 3000 
      } 
    }
  }
}
```

Stats and Extended Stats:
```
{
  "aggs": {
    "price_stats": { "stats": { "field": "price" } },
    "extended": { 
      "extended_stats": { 
        "field": "price",
        "sigma": 2 
      } 
    }
  }
}
```

Percentiles:
```
{
  "aggs": {
    "load_time_percentiles": {
      "percentiles": {
        "field": "load_time",
        "percents": [50, 95, 99],
        "keyed": true,
        "tdigest": { "compression": 100 }
      }
    },
    "load_time_ranks": {
      "percentile_ranks": {
        "field": "load_time",
        "values": [100, 200, 500]
      }
    }
  }
}
```

Top Hits:
```
{
  "aggs": {
    "top_tags": {
      "terms": { "field": "tags.keyword" },
      "aggs": {
        "top_sales_hits": {
          "top_hits": {
            "size": 3,
            "_source": ["title", "price"],
            "sort": [{ "date": "desc" }],
            "highlight": {
              "fields": { "title": {} }
            }
          }
        }
      }
    }
  }
}
```

Scripted Metric:
```
{
  "aggs": {
    "profit": {
      "scripted_metric": {
        "init_script": "state.transactions = []",
        "map_script": "state.transactions.add(doc.revenue.value - doc.cost.value)",
        "combine_script": "double profit = 0; for (t in state.transactions) { profit += t } return profit",
        "reduce_script": "double profit = 0; for (a in states) { profit += a } return profit"
      }
    }
  }
}
```

Weighted Average:
```
{
  "aggs": {
    "weighted_grade": {
      "weighted_avg": {
        "value": { "field": "grade" },
        "weight": { "field": "weight" }
      }
    }
  }
}
```

PIPELINE AGGREGATIONS
------------------------------------------------------------------------------
Derivative:
```
{
  "aggs": {
    "sales_per_month": {
      "date_histogram": { "field": "@timestamp", "calendar_interval": "month" },
      "aggs": {
        "total_sales": { "sum": { "field": "price" } },
        "sales_deriv": { "derivative": { "buckets_path": "total_sales" } }
      }
    }
  }
}
```

Cumulative Sum:
```
{
  "aggs": {
    "sales_per_month": {
      "date_histogram": { "field": "@timestamp", "calendar_interval": "month" },
      "aggs": {
        "sales": { "sum": { "field": "price" } },
        "cumulative_sales": { "cumulative_sum": { "buckets_path": "sales" } }
      }
    }
  }
}
```

Moving Function (moving_fn) - Replaces deprecated moving_avg:
```
{
  "aggs": {
    "daily_sales": {
      "date_histogram": { "field": "@timestamp", "calendar_interval": "day" },
      "aggs": {
        "sales": { "sum": { "field": "price" } },
        "moving_avg_sales": {
          "moving_fn": {
            "buckets_path": "sales",
            "window": 7,
            "script": "MovingFunctions.unweightedAvg(values)"
          }
        }
      }
    }
  }
}
```

Available MovingFunctions:
• MovingFunctions.unweightedAvg(values) - Simple average
• MovingFunctions.linearWeightedAvg(values) - Linear weighted average
• MovingFunctions.ewma(values, alpha) - Exponential weighted moving average
• MovingFunctions.holt(values, alpha, beta) - Holt linear
• MovingFunctions.holtWinters(values, alpha, beta, gamma, period, pad) - Holt-Winters
• MovingFunctions.max(values) / min(values) / sum(values) / stdDev(values)

Bucket Script:
```
{
  "aggs": {
    "sales_per_month": {
      "date_histogram": { "field": "@timestamp", "calendar_interval": "month" },
      "aggs": {
        "total_sales": { "sum": { "field": "price" } },
        "total_cost": { "sum": { "field": "cost" } },
        "profit_margin": {
          "bucket_script": {
            "buckets_path": {
              "sales": "total_sales",
              "cost": "total_cost"
            },
            "script": "(params.sales - params.cost) / params.sales * 100"
          }
        }
      }
    }
  }
}
```

Bucket Selector:
```
{
  "aggs": {
    "sales_per_month": {
      "date_histogram": { "field": "@timestamp", "calendar_interval": "month" },
      "aggs": {
        "total_sales": { "sum": { "field": "price" } },
        "sales_bucket_filter": {
          "bucket_selector": {
            "buckets_path": { "totalSales": "total_sales" },
            "script": "params.totalSales > 1000"
          }
        }
      }
    }
  }
}
```

Bucket Sort:
```
{
  "aggs": {
    "sales_per_month": {
      "date_histogram": { "field": "@timestamp", "calendar_interval": "month" },
      "aggs": {
        "total_sales": { "sum": { "field": "price" } },
        "sales_bucket_sort": {
          "bucket_sort": {
            "sort": [{ "total_sales": { "order": "desc" } }],
            "from": 0,
            "size": 10
          }
        }
      }
    }
  }
}
```

================================================================================
                         EXAM TIPS AND BEST PRACTICES
================================================================================

GENERAL EXAM STRATEGIES
------------------------------------------------------------------------------
1. Time Management:
   - 3 hours for the exam - pace yourself
   - Don't spend too much time on difficult questions
   - Flag questions to review later
   - Save time for verification

2. Environment Familiarity:
   - Practice in a similar Kibana Dev Tools environment
   - Know keyboard shortcuts
   - Understand query formatting

3. Documentation Access:
   - Official documentation is available during exam
   - Know where to find specific topics quickly
   - Practice navigating documentation

4. Common Mistakes to Avoid:
   - Forgetting .keyword suffix for aggregations/sorting
   - Incorrect field paths in nested queries
   - Missing required fields in mappings
   - Wrong query clause (must vs filter)
   - Incorrect date math syntax

PERFORMANCE OPTIMIZATION TIPS
------------------------------------------------------------------------------
1. Query Optimization:
   - Use filter context when scoring not needed
   - Prefer term-level queries for exact matches
   - Avoid leading wildcards
   - Use routing for related documents
   - Limit returned fields with _source filtering

2. Index Design:
   - Right-size shards (20-50GB target)
   - Use appropriate refresh intervals
   - Consider doc values for sorting/aggregations
   - Use index sorting for common sort patterns
   - Optimize mappings (disable unused features)

3. Cluster Performance:
   - Size heap to 50% of RAM (max 32GB)
   - Use dedicated master nodes for large clusters
   - Implement proper shard allocation awareness
   - Monitor and tune thread pools
   - Use circuit breakers appropriately

COMMON API PATTERNS
------------------------------------------------------------------------------
Index Operations:
GET _cat/indices?v
PUT my-index
DELETE my-index
POST my-index/_refresh
POST my-index/_forcemerge
POST my-index/_flush

Mapping Operations:
GET my-index/_mapping
PUT my-index/_mapping { ... }

Search Operations:
GET my-index/_search { ... }
POST my-index/_search { ... }
GET my-index/_count { ... }
GET my-index/_validate/query { ... }

Document Operations:
PUT my-index/_doc/1 { ... }
POST my-index/_doc { ... }
GET my-index/_doc/1
DELETE my-index/_doc/1
POST my-index/_update/1 { ... }

Cluster Operations:
GET _cluster/health
GET _cluster/state
GET _cluster/settings
PUT _cluster/settings { ... }

================================================================================
                         PRACTICE RESOURCES
================================================================================

1. Official Elastic Training:
   - Elastic Training Portal
   - Elasticsearch Engineer I & II courses
   - Hands-on labs

2. Practice Environments:
   - Elastic Cloud free trial
   - Local Docker/Kubernetes deployment
   - Elasticsearch Service on AWS/Azure/GCP

3. Sample Datasets:
   - Sample data in Kibana
   - Elastic Examples repository
   - Kaggle datasets

4. Documentation:
   - Elasticsearch Reference Guide
   - Elasticsearch Definitive Guide
   - Elastic Blog

5. Community Resources:
   - Elastic Community Forums
   - Stack Overflow
   - GitHub Elastic repositories

================================================================================
                         VERSION-SPECIFIC NOTES (8.x)
================================================================================

Key Changes in Elasticsearch 8.x:
1. Security enabled by default (IMPORTANT!)
2. Native support for kNN vector search
3. Runtime fields improvements
4. Improved data streams
5. Simplified deployment
6. Enhanced ML capabilities
7. Text embedding models
8. Semantic search features
9. Improved frozen tier with searchable snapshots
10. Cluster coordination improvements
11. ES|QL - New query language (8.11+)
12. Connectors and Crawler improvements

Security Basics (8.x - Enabled by Default):
```
# Check security status
GET _xpack/security

# Create user
POST _security/user/myuser
{
  "password": "secure_password",
  "roles": ["kibana_admin", "my_custom_role"],
  "full_name": "John Doe"
}

# Create role
POST _security/role/my_custom_role
{
  "cluster": ["monitor"],
  "indices": [
    {
      "names": ["logs-*"],
      "privileges": ["read", "view_index_metadata"]
    }
  ]
}

# Create API key
POST _security/api_key
{
  "name": "my-api-key",
  "expiration": "7d",
  "role_descriptors": {
    "logs_read": {
      "indices": [
        { "names": ["logs-*"], "privileges": ["read"] }
      ]
    }
  }
}
```

Index Settings Limits (Prevent Mapping Explosion):
```
PUT my-index/_settings
{
  "index.mapping.total_fields.limit": 2000,
  "index.mapping.depth.limit": 20,
  "index.mapping.nested_fields.limit": 50,
  "index.mapping.nested_objects.limit": 10000
}
```

Index Sorting (Optimize common queries):
```
PUT my-index
{
  "settings": {
    "index": {
      "sort.field": ["date", "user.keyword"],
      "sort.order": ["desc", "asc"]
    }
  }
}
```

Deprecated/Removed Features:
- types (removed - single type per index)
- _all field (deprecated)
- _all field (deprecated)
- freeze/unfreeze APIs (deprecated)
- mapping type parameter (removed)
- legacy template API (use _index_template)
- boost parameter in mappings (deprecated)

================================================================================
                         QUICK REFERENCE CHEAT SHEET
================================================================================

MOST COMMON EXAM COMMANDS:
--------------------------
// Document Operations
PUT my-index/_doc/1 { }                    // Index with ID
POST my-index/_doc { }                     // Index auto-ID
POST my-index/_update/1 { "doc": { } }     // Update
DELETE my-index/_doc/1                     // Delete
POST _bulk { }                             // Bulk operations
POST _reindex { }                          // Reindex
POST my-index/_update_by_query { }         // Update by query
POST my-index/_delete_by_query { }         // Delete by query

// Index Operations
PUT my-index                               // Create index
DELETE my-index                            // Delete index
GET my-index/_mapping                      // Get mapping
PUT my-index/_mapping { }                  // Update mapping
GET my-index/_settings                     // Get settings
PUT my-index/_settings { }                 // Update settings

// Search Operations
GET my-index/_search { }                   // Search
GET my-index/_count { }                    // Count
GET my-index/_explain/1 { }                // Explain
POST _msearch { }                          // Multi-search
POST my-index/_async_search { }            // Async search

// Cluster Operations
GET _cluster/health                        // Health
GET _cluster/settings                      // Settings
GET _cat/indices?v                         // List indices
GET _cat/shards?v                          // List shards
GET _cat/nodes?v                           // List nodes

// Templates & Pipelines
PUT _index_template/name { }               // Index template
PUT _component_template/name { }           // Component template
PUT _ingest/pipeline/name { }              // Ingest pipeline
POST _ingest/pipeline/_simulate { }        // Simulate pipeline

// Snapshots
PUT _snapshot/repo_name { }                // Register repo
PUT _snapshot/repo/snap_name { }           // Create snapshot
POST _snapshot/repo/snap/_restore { }      // Restore

QUERY TYPE SELECTION GUIDE:
---------------------------
| Use Case                    | Query Type              |
|-----------------------------|-------------------------|
| Exact match (keyword)       | term, terms             |
| Full-text search            | match, multi_match      |
| Phrase search               | match_phrase            |
| Autocomplete                | match_phrase_prefix     |
| Numeric/date range          | range                   |
| Field exists check          | exists                  |
| Pattern matching            | wildcard, regexp        |
| Typo tolerance              | fuzzy, match+fuzziness  |
| Combine conditions          | bool                    |
| Find similar docs           | more_like_this          |
| Nested objects              | nested                  |
| Parent/child                | has_parent, has_child   |
| Geographic                  | geo_distance, geo_shape |
| Vector similarity           | knn                     |

AGGREGATION SELECTION GUIDE:
----------------------------
| Use Case                    | Aggregation Type        |
|-----------------------------|-------------------------|
| Group by category           | terms                   |
| Time-based grouping         | date_histogram          |
| Numeric ranges              | histogram, range        |
| Single value stats          | avg, sum, min, max      |
| Multiple stats              | stats, extended_stats   |
| Unique count                | cardinality             |
| Percentiles                 | percentiles             |
| Top documents per bucket    | top_hits                |
| Filter then aggregate       | filter, filters         |
| Calculate change over time  | derivative              |
| Running totals              | cumulative_sum          |
| Moving averages             | moving_fn               |
| Custom calculations         | bucket_script           |

================================================================================
                         COMMON ERRORS & TROUBLESHOOTING
================================================================================

ERROR: "mapper_parsing_exception"
CAUSE: Document field doesn't match mapping
FIX: Check field type, use ignore_malformed, or update mapping

ERROR: "search_phase_execution_exception" (no mapping for [field.keyword])
CAUSE: Using .keyword on a field that's not text type
FIX: Remove .keyword suffix or check mapping

ERROR: "illegal_argument_exception" (Fielddata disabled on text fields)
CAUSE: Trying to aggregate/sort on text field
FIX: Use field.keyword or enable fielddata (not recommended)

ERROR: "resource_already_exists_exception"
CAUSE: Index already exists
FIX: Delete existing index or use different name

ERROR: "index_not_found_exception"
CAUSE: Index doesn't exist
FIX: Create index first or use ignore_unavailable

ERROR: "version_conflict_engine_exception"
CAUSE: Concurrent updates to same document
FIX: Use retry_on_conflict or conflicts=proceed

ERROR: "circuit_breaking_exception"
CAUSE: Request too large for available memory
FIX: Reduce request size, increase memory, or paginate

ERROR: "cluster_block_exception" (index read-only)
CAUSE: Disk watermark exceeded
FIX: Free disk space, then: PUT my-index/_settings {"index.blocks.read_only_allow_delete": null}

================================================================================
                         PRACTICE SCENARIOS
================================================================================

SCENARIO 1: Log Analytics Setup
- Create index template for logs-* with proper mappings
- Set up ILM policy (hot->warm->cold->delete)
- Create data stream
- Build ingest pipeline for log parsing (grok)
- Create filtered aliases

SCENARIO 2: E-commerce Search
- Design product index with multi-fields
- Implement autocomplete with completion suggester
- Build search with bool query (must/should/filter)
- Add aggregations for faceted navigation
- Implement highlighting

SCENARIO 3: Time-Series Data
- Create transform for hourly summaries
- Set up date_histogram aggregations
- Implement rollover with ILM
- Configure searchable snapshots for cold data

================================================================================
                              END OF GUIDE
================================================================================

Last Updated: February 2026
Elasticsearch Version: 8.x
Certification: Elastic Certified Engineer

Remember: Hands-on practice is the key to passing this exam!
Practice, practice, practice in a real Elasticsearch environment.

FINAL TIP: During the exam, if stuck:
1. Check the documentation (it's allowed!)
2. Use _validate/query to test queries
3. Use _ingest/pipeline/_simulate to test pipelines  
4. Start simple, then add complexity
5. Verify with GET requests before moving on

================================================================================
