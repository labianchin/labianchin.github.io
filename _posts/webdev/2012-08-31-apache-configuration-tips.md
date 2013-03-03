---
layout: post
category : webdev
tags : [apache, php, cakephp]
---

## Improve apache perfomance

Here are some ideas in improving apache performance by adding some lines in a .htaccess file. I used this in a CakePHP project recently.


	<IfModule mod_rewrite.c> #url rewrite used by cakephp
	    RewriteEngine On
	    RewriteCond %{REQUEST_FILENAME} !-d
	    RewriteCond %{REQUEST_FILENAME} !-f
	    RewriteRule ^(.*)$ index.php?url=$1 [QSA,L]
	</IfModule>
	IndexIgnore *
	<IfModule mod_headers.c>
		# 1 YEAR
		<FilesMatch "\.(ico|pdf|flv)$">
		Header set Cache-Control "max-age=29030400, public"
		</FilesMatch>
		# 1 WEEK
		<FilesMatch "\.(jpg|jpeg|png|gif|swf)$">
		Header set Cache-Control "max-age=604800, public"
		</FilesMatch>
		# 2 DAYS
		<FilesMatch "\.(xml|txt|css|js)$">
		Header set Cache-Control "max-age=172800, proxy-revalidate"
		</FilesMatch>
	</IfModule>
	<IfModule mod_deflate.c>
			AddOutputFilterByType DEFLATE text/text text/plain text/css application/x-javascript application/javascript text/javascript
	</IfModule>

The additional lines basically set some cache and compression (mod_deflate) headers for the static content.

More info: 

http://www.howtoforge.com/apache2_mod_deflate

http://www.askapache.com/htaccess/speed-up-sites-with-htaccess-caching.html
