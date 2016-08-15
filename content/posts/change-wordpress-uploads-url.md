---
title: "Change WordPress uploads URL"
date: 2016-08-15T10:38:16-03:00
draft: true
summary: "Changes the URL address WordPress renders for the user-uploaded files. The browser will fetch the files from the new address instead of fetching from the WordPress server."
---

User-uploaded file addresses need to be changed from **http://example.com/wp-content/uploads** to **http://static.example.com/wp-content/uploads**.

```php
update_option(
    'upload_url_path',
    'http://static.example.com/wp-content/uploads'
);
```


## Reference
 * https://developer.wordpress.org/reference/functions/wp_upload_dir/
