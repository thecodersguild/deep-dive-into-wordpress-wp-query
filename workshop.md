#[ WorkShop: Deep Dive into WordPress' WP_Query](http://www.meetup.com/atlanta-wordpress-coders-guild/events/222522372/)
[The Atlanta WordPress Coder's Guild](http://www.meetup.com/atlanta-wordpress-coders-guild/)
- Presenters: [@mikeschinkel](http://twitter.com/mikeschinkel) / [@wpscholar](http://twitter.com/wpscholar)
- Twitter: [@thecodersguild](http://twitter.com/thecodersguild)
- Hashtags: #wp_query #awcgs 

###Setup
- **Presentation:** Just follow along.
- **Workshop:** Set up a child theme:

	- `themes/thecodersguild/style.css`

			/*
			Theme Name: The Coder's Guild
			Template:   twentyfifteen
			Version:    0.1
			*/
			@import url( "../twentyfifteen/style.css" );
	

	- `themes/thecodersguild/functions.php`

			<?php
			
			class TheCodersGuild_Theme {
				static function on_load() {
					// We will add hooks here
				}
			}
			TheCodersGuild_Theme::on_load();
			

###Intro:
- Review The Outline
- What is WP_Query?
	- A PHP class
	- WP's feature to return a list of Posts from the DB
	- **For the Workshop**: 
		- Create a "Meetups" page in WP
		- Copy `page.php` from parent theme to `page-meetups.php`
		- Unzip [EventPress Redux](https://github.com/thecodersguild/wordpress-plugins-and-hooks/tree/master/mu-plugins) into `mu-plugins`
		- Add `plugin-loader.php` into `mu-plugins`
		- Add _"Camp Style"_, _"Conference"_ and _"Meetup"_ to _Events > Event Type_
		- Add Venues:
			- StrongBox West
			- The Loudermilk Center
			- The Westin Lake Las Vegas
			- Pennsylvania Convention Center
		- Add Events with:
			- 2 AWCG Meetups held at StrongBox West: Plugins 10-28-2005 & WP_Query 11-18-2005 
			- 1 Camp Style held at The Loudermilk Center: Atlanta WordCamp  March 27-29, 2015
			- 1 Conference held at The Westin Lake Las Vegas: LoopConf  May 4-6, 2015
			- 1 Conference + Camp Style held at Pennsylvania Convention Center: WordCamp US Dec 4-6, 2015

### Overview of WP_Query usage
- **For the Workshop**: 
	- Go to https://generatewp.com/wp_query/
- What does `WP_Query` allow querying?
	- Post fields
		- `ID`, `post_name`, `post_type`, `post_parent`, `post_author`, date fields, etc.
		- Freeform text match in content
	- Post Meta
	- Related Taxonomy Terms
	- Author Names

- `WP_Query` allows Subsetting and Ordering
	- Pagination
	- Order
	
- `WP_Query` allow Hiding and Caching
	- Permissions
	- Caching 

- References
	- https://codex.wordpress.org/Class_Reference/WP_Query
	- http://code.tutsplus.com/series/mastering-wp_query--cms-818
	- https://generatewp.com/wp_query/
	

###Basic WP_Query Usage	
- With [The Loop](https://codex.wordpress.org/The_Loop): 
	
		$query = new WP_Query( array( 'post_type' => 'epr_event' ) );
		while ( $query->have_posts() ) : $query->the_post();
			echo '<h3>' . get_the_title( $query->post ) . '</h3>';
		endwhile;			

- Using directly: 
	
		$query = new WP_Query( array( 'post_type' => 'epr_event' ) );
		foreach( $query->posts as $post ):
			echo '<h3>' . get_the_title( $post ) . '</h3>';
		endforeach;
		
- Automatically used by WordPress on every page load:

		global $wp_query, $wp_the_query	
			
- Vs. `query_posts()` and `get_posts()`

	- `$posts = query_posts()`: **Not recommended**
		
			query_posts( array( 'post-type' => 'page' )  );
			// You can now do the loop here

			
	- `get_posts()`: **Ok**
	
			$posts = get_posts( array( 'post-type' => 'page' )  );
			
- Using `wp_reset_postdata()`
	- Use after using "The Loop"
	- Better: **Don't use The Loop!**


### Intermediate WP_Query Usage

- Query by Taxonomy Terms:
	- WP_Query parameter: 
		- `tax_query`
	    - **For the Workshop**: 
	        - Add the following parameter to _inside_ array passed to `WP_Query` in `page-meetup.php`:
    
			 	    'tax_query' => array(
				        array(
					        'taxonomy' => 'epr_event_type',
					        'field'    => 'slug',
					        'terms'    => 'meetup',
				        ),
			        ),
    
	- `tax_query` parameter:
	    - **For the Workshop**: 
	        - Change the inside of the array passed to `WP_Query` in `page-meetup.php` to:
    
		            'tax_query' => array(
			            'relation' => 'AND',
			            array(
				            'taxonomy' => 'epr_event_type',
				            'field'    => 'slug',
				            'terms'    => 'conference',
			            ),
			            array(
				            'taxonomy' => 'epr_event_type',
				            'field'    => 'slug',
				            'terms'    => 'camp-style',
			            ),
		            ),



- Query by Meta Fields

	- `meta_key`, `meta_value`, `meta_compare`
	    - **For the Workshop**: 
	        - Replace the `tax_query` in `page-meetup.php` with:
    
			    	'meta_key'     => '_epr_event[venue_id]',
			    	'meta_value'   => 19,
			    	'meta_compare' => '=',



	
		

	- `meta_query`
	    - **For the Workshop**: 
	    
	        - Replace the `meta_***` args in `page-meetup.php` with:

		        	'meta_query' => array(
					    'relation' => 'OR',
					    array(
						    'key' 	 	=> _'epr_event[venue_id]',
					    	'value'   	=> 19,
						    'compare'   	=> '=',
					    ),
					    array(
						    'key' 	 	=> _'epr_event[venue_id]',
						    'value'   	=> 44,
						    'compare'   	=> '=',
					    ),
				    ),

	    - **For the Workshop**: 
       
	        - Replace the `meta_query` args in `page-meetup.php` with:
       
	    			$sbw = new WP_Query( array(
						'fields' => 'ids',
       		         	'post_type' => 'epr_venue',
       	    	     	'name' => 'strongbox-west'
       	       		));
            
					$tlc = new WP_Query( array(
						'fields' => 'ids',
						'post_type' => 'epr_venue',
						'name' => 'the-loudermilk-center'
					));
            
					$query = new WP_Query( array(
						'post_type' => 'epr_event',
						'meta_query' => array(
							'key'      => '_epr_event[venue_id]',
							'compare'   => 'IN',
							'value'   	=> array(
								$sbw->posts[0],
								$tlc->posts[0],
							),
						),
					));
	
- Using Hooks
    - `'pre_get_posts'`
    - `'posts_clauses'`


### Advanced WP_Query Usage
- 

- Optimization
	- `$nopaging`
	- `$no_found_rows` 
		- Found rows not needed for `$nopaging`
		- Might be needed for custom code.



- Modifying SQL
	- When do you need to modify SQL
	- Use `is_main_query()` when needed
	- Always modify query based on using `$args`
	- Using `$wpdb` 
		- Table names
		- `prepare()`

- Caching Queries
	- Using Transients
	- Using Object Cache

- Setting 404

		$wp_query->set_404();
		status_header( 404 );
		nocache_headers();
	
- Best Practice
	- Always modify query based on an argument
		- Not based on globals or other implicit state


### Other Hook List

8. More Specific Hooks
    - `'posts_where'`	
    - `'posts_join'`	
    - `'posts_orderby'`
    - `'posts_distinct'`
    - `'post_limits'`
    - `'posts_fields'`
    - `'posts_groupby'`

9. Request Hooks
    - `'posts_where_request'`
    - `'posts_groupby_request'`
    - `'posts_join_request'`
    - `'posts_orderby_request'`
    - `'posts_distinct_request'`
    - `'post_limits_request'`
    - `'posts_fields_request'`
    - `'posts_clauses_request'`

9. Other Hooks
    - `'posts_selection'`
    - `'parse_tax_query'`
    - `'posts_search'`
    - `'posts_search_orderby'`
    - `'comment_feed_join'`
    - `'comment_feed_where'`
    - `'comment_feed_groupby'`
    - `'comment_feed_orderby'`
    - `'comment_feed_limits'`
    - `'posts_where_paged'`
    - `'posts_join_paged'`
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


















