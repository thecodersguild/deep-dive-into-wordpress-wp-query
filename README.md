#Deep Dive into WordPress' WP_Query
The Atlanta WordPress Coder's Guild
- Presenters: @mikeschinkel / @wpscholar
- Twitter: @thecodersguild
- Hashtags: #wp_query #awcgs 


##Outline
0. Intro
	- What is `WP_Query`?
	- We'll gloss over 
	- We'll dive in deep
	
1. Overview of `WP_Query` usage
	- https://codex.wordpress.org/Class_Reference/WP_Query
	- https://generatewp.com/wp_query/
	- What does `WP_Query` allow querying?
		- Post fields
			- ID, post_name, post_type, post_parent, post_author, date fields, etc.
			- Freeform text match in content
		- Post Meta
		- Related taxonomy terms
		- Author Names
		- Pagination
		- Order
	- Arguments that do not affect SQL query
		- Permissions
		- Caching 
		
2. Basic `WP_Query` Usage
	- Default query configuration
		- `$post_status='public'`
	- Query by common: `ID`, `post_type`, etc.
	
3. Query by Meta Fields
	- `meta_query`

4. Query by Taxonomy Terms: 
	- `tax_query`
	
5. Modifying SQL
	- When do you need to modify SQL
	- Use `is_main_query()` when needed
	- Always modify query based on using `$args`

5. Caching Queries
	- Using Transients
	- Using Object Cache

6. Most Used Hooks
    - `'posts_clauses'`
    - `'pre_get_posts'`

6. Other Hooks
    - `'posts_selection'`
    - `'parse_tax_query'`
    - `'posts_search'`
    - `'posts_search_orderby'`
    - `'posts_where'`	
    - `'posts_join'`	
    - `'comment_feed_join'`
    - `'comment_feed_where'`
    - `'comment_feed_groupby'`
    - `'comment_feed_orderby'`
    - `'comment_feed_limits'`
    - `'posts_where_paged'`
    - `'posts_groupby'`
    - `'posts_join_paged'`
    - `'posts_orderby'`
    - `'posts_distinct'`
    - `'post_limits'`
    - `'posts_fields'`
    - `'posts_where_request'`
    - `'posts_groupby_request'`
    - `'posts_join_request'`
    - `'posts_orderby_request'`
    - `'posts_distinct_request'`
    - `'post_limits_request'`
    - `'posts_fields_request'`
    - `'posts_clauses_request'`
    - `'posts_request'`
    - `'split_the_query'`
    - `'posts_request_ids'`
    - `'posts_results'`
    - `'the_preview'`
    - `'the_posts'`
    - `'found_posts_query'`
    - `'found_posts'`
    - `'content_pagination'`
    - `'old_slug_redirect_url'`
    - `'wp_search_stopwords'`



