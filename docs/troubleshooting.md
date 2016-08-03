---
title: "Troubleshooting"
slug: "troubleshooting.md"
weight: 30
---

## 1) MySQL error with PRIMARY KEY must be NOT NULL

Reference: https://github.com/rails/rails/pull/13247#issuecomment-158787912

Error looks like.

```
...
rake aborted!
ActiveRecord::StatementInvalid: Mysql2::Error: All parts of a PRIMARY KEY must be NOT NULL; if you need NULL in a key, use UNIQUE instead: CREATE TABLE `activities` (`id` int(11) DEFAULT NULL auto_increment PRIMARY KEY, `activity_verb_id` int(11), `created_at` datetime, `updated_at` datetime, `ancestry` varchar(255), `author_id` int(11), `user_author_id` int(11), `owner_id` int(11)) ENGINE=InnoDB
```

### Solution

The project uses an old version of MySQL. Either you downgrade your
MySQL or you can add the file
`config/initializers/abstract_mysql2_adapter.rb` with the following
content to avoid this error.

```ruby
class ActiveRecord::ConnectionAdapters::Mysql2Adapter
  NATIVE_DATABASE_TYPES[:primary_key] = "int(11) auto_increment PRIMARY KEY"
end
```

## 2) MySQL error with BLOB, TEXT, GEOMETRY or JSON column 'xxx' can't have a default value

Reference: http://stackoverflow.com/questions/3466872/why-cant-a-text-column-have-a-default-value-in-mysql


Error looks like.

```
...
rake aborted!
ActiveRecord::StatementInvalid: Mysql2::Error: BLOB, TEXT, GEOMETRY or JSON column 'tag_array_text' can't have a default value: CREATE TABLE `activity_objects` (`id` int(11) auto_increment PRIMARY KEY, `created_at` datetime, `updated_at` datetime, `object_type` varchar(45), `like_count` int(11) DEFAULT 0, `title` varchar(255) DEFAULT '', `description` text, `follower_count` int(11) DEFAULT 0, `visit_count` int(11) DEFAULT 0, `language` varchar(255), `age_min` int(11) DEFAULT 0, `age_max` int(11) DEFAULT 0, `notified_after_draft` tinyint(1) DEFAULT 0, `comment_count` int(11) DEFAULT 0, `popularity` int(11) DEFAULT 0, `download_count` int(11) DEFAULT 0, `qscore` int(11) DEFAULT 500000, `reviewers_qscore` decimal(12,6), `users_qscore` decimal(12,6), `ranking` int(11) DEFAULT 0, `title_length` int(11) DEFAULT 1, `desc_length` int(11) DEFAULT 1, `tags_length` int(11) DEFAULT 1, `scope` int(11) DEFAULT 0, `avatar_file_name` varchar(255), `avatar_content_type` varchar(255), `avatar_file_size` int(11), `avatar_updated_at` datetime, `teachers_qscore` decimal(12,6), `license_id` int(11), `original_author` text, `license_attribution` text, `license_custom` text, `metadata_qscore` decimal(12,6) DEFAULT 0.0, `allow_download` tinyint(1) DEFAULT 1, `allow_comment` tinyint(1) DEFAULT 1, `allow_clone` tinyint(1) DEFAULT 1, `competition` tinyint(1) DEFAULT 0, `tag_array_text` text DEFAULT '', `interaction_qscore` decimal(12,6)) ENGINE=InnoDB
```

### Solution

The project uses an old version of MySQL. Either you downgrade your
MySQL or you can run in MySQL the following command:

```
set @@global.sql_mode = "MYSQL40";
```

## 3) Sphinx error with index 'xxxx': id is not a valid attribute name

Error looks like.

```
...
Unsupported version: 2.2.10

For more information, read the documentation:
http://pat.github.com/ts/en/advanced_config.html

Generating Configuration to /.../config/development.sphinx.conf
Sphinx 2.2.10-id64-release (2c212e0)
Copyright (c) 2001-2015, Andrew Aksyonoff
Copyright (c) 2008-2015, Sphinx Technologies Inc (http://sphinxsearch.com)

using config file '/.../config/development.sphinx.conf'...
WARNING: key 'address' was permanently removed from Sphinx configuration. Refer to documentation for details.
WARNING: key 'port' was permanently removed from Sphinx configuration. Refer to documentation for details.
WARNING: key 'max_matches' was permanently removed from Sphinx configuration. Refer to documentation for details.
WARNING: key 'sql_query_info' was permanently removed from Sphinx configuration. Refer to documentation for details.
WARNING: key 'charset_type' was permanently removed from Sphinx configuration. Refer to documentation for details.
WARNING: 59 more warnings skipped.
indexing index 'audio_core'...
ERROR: index 'audio_core': id is not a valid attribute name.
```

### Solution

The project uses an old version of Sphinx.
Make sure you install version 2.1.* (successfully tested with 2.1.9).


## 4) Sphinx error with Failed to start searchd daemon

Error looks like.

```
...
skipping non-plain index 'writing'...
total 19 reads, 0.000 sec, 0.0 kb/call avg, 0.0 msec/call avg
total 114 writes, 0.001 sec, 1.0 kb/call avg, 0.0 msec/call avg
Failed to start searchd daemon. Check /.../log/searchd.log.
Failed to start searchd daemon. Check /.../log/searchd.log
Be sure to run thinking_sphinx:index before thinking_sphinx:start
```

### Solution

Make sure that there is no other searchd running.

## 5) Sphinx error with cannot be found on your system.

Error looks like.

```
...
Sphinx cannot be found on your system. You may need to configure the following
settings in your config/sphinx.yml file:
  * bin_path
  * searchd_binary_name
  * indexer_binary_name


For more information, read the documentation:
http://pat.github.com/ts/en/advanced_config.html

```

### Solution

Sphinx configuration file `config/sphinx.yml` have a wrong attribute,
change `bin-path` to `bin_path` if you want to change the binaries
path.

