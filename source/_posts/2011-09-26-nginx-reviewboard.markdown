---
layout: post
title: "ReviewBoard with Nginx"
date: 2011-09-26 08:50
comments: true
categories: nginx, reviewboard
---

This weekend I decided to checkout ReviewBoard a slick Web 2.0 open source Django app for doing code reviews. Unfortunately, it took me quite a bit of time to set it up from scratch with Nginx. Thus, I decided to do a quick writeup for Nginx users.
ReviewBoard's setup docs is a good starting point for getting the dependencies you need to setup a ReviewBoard website.  

Once you have ReviewBoard installed you can generate a website using `rb-site install`. I setup my website under /var/www/review.mysite.com but you can set it up anywhere if you prefer another location. 
The next step is to launch the FastCGI daemon script bundled with ReviewBoard

{% codeblock %}
rb-site manage /var/www/review runfcgi method=threaded port=3033 host=127.0.0.1 protocol=fcgi 
{% endcodeblock %}

Once your FastCGI instance is up and running you just need to simply point Nginx at the FastCGI process

{% codeblock %}
server {
    listen 80;    
    server_name review.mysite.com;
    root /var/www/review.mysite.com/htdocs/;    
    
    location /media  {     
      root /htdocs/media/;    
    }
   
    location /errordoc {      
      root /htdocs/errordocs/;    
    }

    location / {      
      # host and port to fastcgi server      
      fastcgi_pass 127.0.0.1:3033;      
      fastcgi_param PATH_INFO $fastcgi_script_name;      
      fastcgi_param REQUEST_METHOD $request_method;      
      fastcgi_param QUERY_STRING $query_string;      
      fastcgi_param CONTENT_TYPE $content_type;      
      fastcgi_param CONTENT_LENGTH $content_length;      
      fastcgi_pass_header Authorization;      
      fastcgi_intercept_errors off;    
    }  
}
{% endcodeblock %}

Then restart your nginx server and reviewboard should be up and running. Did I miss anything?
